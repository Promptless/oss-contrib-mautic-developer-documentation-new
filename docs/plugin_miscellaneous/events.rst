Events
######

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Mautic uses Symfony's EventDispatcher to run and communicate actions throughout the app. Plugins hook into these events to extend Mautic's capabilities.

Subscribers
***********

The easiest way to listen to events is with an event subscriber. Implement ``Symfony\Component\EventDispatcher\EventSubscriberInterface`` and declare the events you want to handle in ``getSubscribedEvents()``. Inject any dependencies your subscriber needs through the constructor:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/EventListener/LeadSubscriber.php

    namespace MauticPlugin\HelloWorldBundle\EventListener;

    use Mautic\LeadBundle\Event\LeadEvent;
    use Mautic\LeadBundle\LeadEvents;
    use Symfony\Component\EventDispatcher\EventSubscriberInterface;

    class LeadSubscriber implements EventSubscriberInterface
    {
        public static function getSubscribedEvents(): array
        {
            return [
                LeadEvents::LEAD_POST_SAVE   => ['onLeadPostSave', 0],
                LeadEvents::LEAD_POST_DELETE => ['onLeadDelete', 0],
            ];
        }

        public function onLeadPostSave(LeadEvent $event): void
        {
            $lead = $event->getLead();

            // Do something.
        }

        public function onLeadDelete(LeadEvent $event): void
        {
            $lead = $event->getLead();

            // Do something.
        }
    }

Mautic tags each class that implements ``EventSubscriberInterface`` as an event subscriber automatically, so no manual service configuration is required.

Available events
****************

Mautic defines many events across its bundles. To find the ones you can listen to, look at the ``*Events.php`` file in the root of the relevant bundle. For example, Contact-related events are defined in ``app/bundles/LeadBundle/LeadEvents.php``. Always reference the event constants rather than the string names, so future changes to event names don't break your Plugin.

Custom events
*************

A Plugin can create and dispatch its own events. A custom event requires three things.

First, a ``final`` class that defines the available event names as constants:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/HelloWorldEvents.php

    namespace MauticPlugin\HelloWorldBundle;

    final class HelloWorldEvents
    {
        /**
         * The helloworld.armageddon event is dispatched when a world is doomed by a giant meteor.
         *
         * The event listener receives a MauticPlugin\HelloWorldBundle\Event\ArmageddonEvent instance.
         */
        public const ARMAGEDDON = 'helloworld.armageddon';
    }

Second, the event class that listeners receive. It extends ``Symfony\Contracts\EventDispatcher\Event`` and carries the information listeners need:

.. code-block:: php

    <?php
    // plugins/HelloWorldBundle/Event/ArmageddonEvent.php

    namespace MauticPlugin\HelloWorldBundle\Event;

    use MauticPlugin\HelloWorldBundle\Entity\World;
    use Symfony\Contracts\EventDispatcher\Event;

    class ArmageddonEvent extends Event
    {
        public function __construct(private World $world)
        {
        }

        public function shouldPanic(): bool
        {
            return 'earth' === $this->world->getName();
        }
    }

Third, the code that dispatches the event where appropriate, using the :doc:`event dispatcher</plugin_services/event_dispatcher>`:

.. code-block:: php

    <?php

    $event = $this->dispatcher->dispatch(
        new ArmageddonEvent($world),
        HelloWorldEvents::ARMAGEDDON
    );

    if ($event->shouldPanic()) {
        throw new \RuntimeException('Run for the hills!');
    }
