Implementing translation support to entities
############################################

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Mautic provides helper interfaces and traits to add translated-content support to an entity.

``Mautic\CoreBundle\Entity\TranslationInterface``
    This interface ensures an entity has everything Mautic needs to handle translations correctly.

``Mautic\CoreBundle\Entity\TranslationEntityTrait``
    This trait provides the properties that define an entity's language and its relationships to other items. In the entity's ``loadMetadata()`` method, call ``$this->addTranslationMetadata()``.

``Mautic\CoreBundle\Entity\TranslationModelTrait``
    This trait provides ``getTranslatedEntity()``, which determines the entity to use as the translation based on the Contact and/or the ``HTTP_ACCEPT_LANGUAGE`` header. It also provides ``postTranslationEntitySave()``, which you call at the end of the entity's ``saveEntity()`` method.

``Mautic\CoreBundle\Doctrine\TranslationMigrationTrait``
    This trait helps generate schema that matches the entity. Call ``$this->addTranslationSchema()`` to use it.

Translated entity form
**********************

Add ``language`` and ``translationParent`` fields to the entity's form type, using an ``IdToEntityModelTransformer`` to convert the parent ID to an entity:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/Form/Type/WorldType.php

    use Mautic\CoreBundle\Form\Transformer\IdToEntityModelTransformer;

    $transformer = new IdToEntityModelTransformer($this->entityManager, World::class);

    $builder->add(
        $builder->create('translationParent', WorldListType::class, [
            'label'       => 'mautic.core.form.translation_parent',
            'required'    => false,
            'multiple'    => false,
            'ignore_ids'  => [(int) $options['data']->getId()],
        ])->addModelTransformer($transformer)
    );

    $builder->add('language', LocaleType::class, [
        'label'      => 'mautic.core.language',
        'required'   => false,
        'empty_data' => 'en',
    ]);
