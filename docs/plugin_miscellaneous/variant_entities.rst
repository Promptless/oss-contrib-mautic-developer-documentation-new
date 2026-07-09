Implementing variant - A/B test - support to entities
#####################################################

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Mautic provides helper interfaces and traits to add variant support to an entity. This is particularly useful for A/B testing.

``Mautic\CoreBundle\Entity\VariantInterface``
    This interface ensures an entity has everything Mautic needs to handle variants correctly.

``Mautic\CoreBundle\Entity\VariantEntityTrait``
    This trait provides the properties that define an entity's relationship to its variants. In the entity's ``loadMetadata()`` method, call ``$this->addVariantMetadata()``.

``Mautic\CoreBundle\Entity\VariantModelTrait``
    This trait provides ``preVariantSaveEntity()``, ``postVariantSaveEntity()``, and ``convertVariant()``. Call ``preVariantSaveEntity()`` before ``saveEntity()`` and ``postVariantSaveEntity()`` after it.

``Mautic\CoreBundle\Doctrine\VariantMigrationTrait``
    This trait helps generate schema that matches the entity. Call ``$this->addVariantSchema()`` to use it.

The following example shows the save sequence in a model:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/Model/WorldModel.php

    // Reset the A/B test if applicable.
    $variantStartDate = new \DateTime();

    // 'setVariantHits' is the stat-tracker property for this variant.
    $resetVariants = $this->preVariantSaveEntity($entity, ['setVariantHits'], $variantStartDate);

    parent::saveEntity($entity, $unlock);

    $this->postVariantSaveEntity($entity, $resetVariants, $entity->getRelatedEntityIds(), $variantStartDate);

Variant entity form
*******************

Add a ``variantParent`` field to the entity's form type, using an ``IdToEntityModelTransformer`` to convert the parent ID to an entity. In this example the field is hidden and its value is set in the controller when an 'Add A/B Test' button is selected; your Plugin may need a select list instead:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/Form/Type/WorldType.php

    use Mautic\CoreBundle\Form\Transformer\IdToEntityModelTransformer;
    use Symfony\Component\Form\Extension\Core\Type\HiddenType;

    $transformer = new IdToEntityModelTransformer($this->entityManager, World::class);

    $builder->add(
        $builder->create('variantParent', HiddenType::class)
            ->addModelTransformer($transformer)
    );
