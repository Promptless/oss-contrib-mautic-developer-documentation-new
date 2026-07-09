Manipulating Contacts
#####################

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Many Plugins that extend Mautic manipulate Contacts in one way or another. This page summarizes how to obtain the current Contact and work with Contact data.

.. note::

   Contacts were originally called "leads." Much of the code still refers to Contacts as leads - for example, the ``lead`` model key and the ``Lead`` entity.

Obtain the ``Mautic\LeadBundle\Model\LeadModel`` to read and set the currently tracked Contact. Inject it directly into your service, or use the ``getModel('lead')`` helper in a controller:

.. code-block:: php

    <?php

    use Mautic\LeadBundle\Entity\Lead;
    use Mautic\LeadBundle\Model\LeadModel;

    /** @var LeadModel $leadModel */
    $leadModel = $this->getModel('lead');

    /** @var Lead $currentLead */
    $currentLead = $leadModel->getCurrentLead();

    // Pass true to obtain the tracking ID as well.
    [$currentLead, $trackingId, $trackingIdIsNewlyGenerated] = $leadModel->getCurrentLead(true);

    // Set the currently tracked Contact and generate tracking cookies.
    $lead = new Lead();
    // ...
    $leadModel->setCurrentLead($lead);

    // Set a Contact for system use - for events that call getCurrentLead() - without generating tracking cookies.
    $leadModel->setSystemCurrentLead($lead);

Contact tracking
****************

Contacts are tracked by two cookies. The first records the ID that Mautic tracks the Contact under. The second tracks the Contact's activity for the current session; it defaults to 30 minutes and resets on each interaction. ``mautic_session_id`` holds the Contact's current session ID, which in turn names the cookie that holds the Contact's ID.

A cookie named ``mtc_id`` is also placed on any domain with ``mtc.js`` embedded that is allowed by Mautic's CORS settings. It contains the ID of the currently tracked Contact.

Creating new Contacts
*********************

To create a new Contact, use the ``Mautic\LeadBundle\Entity\Lead`` entity. Before saving, you can inspect the unique identifier fields to detect and merge duplicate Contacts:

.. code-block:: php

    <?php

    // Generate a completely new Contact.
    $lead = new Lead();
    $lead->setNewlyCreated(true);

    // The new or updated field values.
    $leadFields = [
        'firstname' => 'Bob',
        // ...
    ];

    // Optionally check identifier fields to determine whether the Contact is unique.
    $uniqueLeadFields    = $this->getModel('lead.field')->getUniqueIdentifierFields();
    $uniqueLeadFieldData = array_intersect_key($leadFields, $uniqueLeadFields);

    if ($uniqueLeadFieldData) {
        $existingLeads = $this->entityManager
            ->getRepository(Lead::class)
            ->getLeadsByUniqueFields($uniqueLeadFieldData);

        if (!empty($existingLeads)) {
            // A match was found, so merge the two Contacts.
            $lead = $leadModel->mergeLeads($lead, $existingLeads[0]);
        }
    }

    // Set the Contact's data.
    $leadModel->setFieldValues($lead, $leadFields);

    // Save the entity.
    $leadModel->saveEntity($lead);

    // Set the updated Contact as the currently tracked one.
    $leadModel->setCurrentLead($lead);
