Forms
#####

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Mautic builds on Symfony's Form component and its Form Type classes. For a general overview, refer to :xref:`Symfony form classes`.

.. note::

   For more complex use cases - such as custom Form Types, dynamic Form modification, and Form validation - see :doc:`/plugin_extensions/forms_advanced`.

.. vale off

Form Types
**********

.. vale on

Form Type classes are the recommended way to define Forms. Mautic lets you register :xref:`Form type services<Symfony custom Form field type>` through the bundle's config file, so you can inject dependencies and reference the type by its class name.

Data sanitizing
***************

Mautic doesn't sanitize Form data automatically. Instead, it provides a Form event subscriber to handle this. In your :xref:`Form type class<Symfony form classes>`, register the ``Mautic\CoreBundle\Form\EventListener\CleanFormSubscriber`` event subscriber in the ``buildForm()`` method:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/Form/Type/WorldType.php

    use Mautic\CoreBundle\Form\EventListener\CleanFormSubscriber;
    use Symfony\Component\Form\FormBuilderInterface;

    // ...
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder->addEventSubscriber(
            new CleanFormSubscriber([
                'content'    => 'html',
                'customHtml' => 'html',
            ])
        );

        // ...
    }

In the array passed to ``CleanFormSubscriber``, the keys are Form Field names and the values are the masks used to sanitize each field. Any Form Field you don't specify uses the ``clean`` mask by default.

.. vale off

Manipulating Forms
******************

.. vale on

When a Form needs to change based on submitted data - for example, changing defined fields, adjusting constraints, or changing select choices - use a Form event listener. See Symfony's documentation on this: :xref:`Symfony dynamic form modification`.

Validating data
***************

For a general overview, review Symfony's Form validation documentation: :xref:`Symfony form validation`. There are two common ways to validate Form data.

Using entity static callback
============================

If the underlying data of a Form is an Entity object, define a static ``loadValidatorMetadata()`` method in the Entity class. Symfony calls it automatically while processing Form data:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/Entity/World.php

    use Symfony\Component\Validator\Constraints\NotBlank;
    use Symfony\Component\Validator\Mapping\ClassMetadata;

    // ...
    public static function loadValidatorMetadata(ClassMetadata $metadata): void
    {
        $metadata->addPropertyConstraint(
            'name',
            new NotBlank(['message' => 'mautic.core.name.required'])
        );
    }

A Form can also use :xref:`validation_groups<Symfony 5 Form validation groups>` to control which constraints apply. Register a static callback in the Form Type class to decide which validation groups Symfony uses for a given submission.

Using constraints
=================

A :xref:`Form type service<Symfony custom Form field type>` can also register :xref:`Constraints<Symfony Form constraints>` directly on a field when building the Form:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/Form/Type/WorldType.php

    use Symfony\Component\Form\Extension\Core\Type\TextType;
    use Symfony\Component\Validator\Constraints\NotBlank;

    // ...
    public function buildForm(FormBuilderInterface $builder, array $options): void
    {
        $builder->add('name', TextType::class, [
            'label'       => 'mautic.core.name',
            'constraints' => [
                new NotBlank(['message' => 'mautic.core.value.required']),
            ],
        ]);
    }
