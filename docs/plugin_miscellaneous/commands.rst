Commands
########

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Add new command-line commands to your Plugin using `Symfony's console component <https://symfony.com/doc/current/console.html>`_. Place command classes in your bundle's ``Command`` directory; Mautic registers them as console commands automatically.

Moderated commands
******************

Some commands should only ever run one instance at a time - for example, scheduled broadcasts. Extend ``Mautic\CoreBundle\Command\ModeratedCommand`` to get this behavior:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/Command/WorldCommand.php

    namespace MauticPlugin\HelloWorldBundle\Command;

    use Mautic\CoreBundle\Command\ModeratedCommand;
    use Symfony\Component\Console\Input\InputInterface;
    use Symfony\Component\Console\Output\OutputInterface;

    class WorldCommand extends ModeratedCommand
    {
        protected function configure(): void
        {
            // ...

            // Append the options used by ModeratedCommand
            parent::configure();
        }

        protected function execute(InputInterface $input, OutputInterface $output): int
        {
            // Stop if another instance is already running.
            // Pass a third argument if this command runs something unique, such as an ID.
            if (!$this->checkRunStatus($input, $output)) {
                return 0;
            }

            // Do the work.

            // Complete this run
            $this->completeRun();

            return 0;
        }
    }

``ModeratedCommand`` allows only one instance of the command to run at a time. Call ``checkRunStatus()`` at the start of ``execute()`` and ``completeRun()`` when the work finishes.
