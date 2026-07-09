Flash messages
##############

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Flash messages - the alerts Mautic shows after an action - are managed by the ``Mautic\CoreBundle\Service\FlashBag`` service. It handles translation and message levels, and can optionally add a matching entry to the notification center.

From a controller
*****************

.. vale off

Controllers that extend one of :ref:`Mautic's common controllers<plugins/mvc:Controllers>` can use the ``addFlash()`` helper:

.. vale on

.. code-block:: php

    <?php

    $this->addFlash(
        'mautic.translation.key',
        ['%placeholder%' => 'some text'],
        'notice',   // Notification type
        'flashes'   // Translation domain
    );

From a service
**************

From a model or any other service, inject ``Mautic\CoreBundle\Service\FlashBag`` and call ``add()``:

.. code-block:: php

    <?php

    use Mautic\CoreBundle\Service\FlashBag;

    final class ExampleService
    {
        public function __construct(private FlashBag $flashBag)
        {
        }

        public function notify(): void
        {
            $this->flashBag->add(
                'mautic.translation.key',
                ['%placeholder%' => 'some text'],
                FlashBag::LEVEL_NOTICE,
                'flashes'
            );
        }
    }

Notifications
*************

Mautic also has a notification center. By default, adding a flash message also adds a notification, but you can add one manually. Controllers can use the ``addNotification()`` helper, while models and other services can inject the notification model - ``Mautic\CoreBundle\Model\NotificationModel`` - and call ``addNotification()`` on it.
