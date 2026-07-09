Model factory
#############

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Use ``Mautic\CoreBundle\Factory\ModelFactory`` when the model a service needs isn't known until runtime. This is useful when the channel and channel ID are stored in the database and the executing code has to look up information about the channel entity. Inject the factory into your service:

.. code-block:: php

    <?php

    use Mautic\CoreBundle\Factory\ModelFactory;

    final class ExampleService
    {
        public function __construct(private ModelFactory $modelFactory)
        {
        }

        public function describeChannelEntity(string $channel, int $channelId): void
        {
            if ($this->modelFactory->hasModel($channel)) {
                $model = $this->modelFactory->getModel($channel);

                if ($entity = $model->getEntity($channelId)) {
                    echo $entity->getName();
                }
            }
        }
    }

The factory has two methods: ``hasModel(string $modelNameKey)`` checks whether a model exists, and ``getModel(string $modelNameKey)`` returns it. Both accept the same key format as the controller ``getModel()`` helper - for example, ``email`` resolves to ``Mautic\EmailBundle\Model\EmailModel``.

.. tip::

   When you know the model at build time, inject the model class directly instead of resolving it through the factory.
