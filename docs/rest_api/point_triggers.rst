Point Triggers
##############

.. vale off

.. note::

  The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

  If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

  Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Use this endpoint to obtain details on Mautic's Point Triggers.

**Using Mautic's API Library**

You can interact with this API through the :xref:`Mautic API Library` as follows, or use the various http endpoints as described in this document.

.. code-block:: php

   <?php
   use Mautic\MauticApi;
   use Mautic\Auth\ApiAuth;

   // ...
   $initAuth   = new ApiAuth();
   $auth       = $initAuth->newAuth($settings);
   $apiUrl     = "https://example.com";
   $api        = new MauticApi();
   $triggerApi = $api->newApi("pointTriggers", $auth, $apiUrl);

.. vale off

Get Point Trigger
*****************

.. vale on

.. code-block:: php

   <?php

   //...
   $trigger = $triggerApi->get($id);

.. code-block:: json

   {
       "trigger": {
           "id": 1,
           "name": "Trigger test",
           "description": null,
           "category": null,
           "isPublished": true,
           "publishUp": null,
           "publishDown": null,
           "dateAdded": "2015-07-23T03:20:42-05:00",
           "createdBy": 1,
           "createdByUser": "Joe Smith",
           "dateModified": null,
           "modifiedBy": null,
           "modifiedByUser": null,
           "points": 10,
           "color": "ab5959",
           "events": {
               "1": {
                   "id": 1,
                   "type": "email.send",
                   "name": "Send email",
                   "description": null,
                   "order": 1,
                   "properties": {
                       "email": 21
                   }
               }
           }
       }
   }

Get an individual Point Trigger by ID.

.. vale off

**HTTP Request**

.. vale on

``GET /points/triggers/ID``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

See JSON code example.

**Point Trigger Properties**

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Point Trigger
   * - ``name``
     - string
     - Name of the Point Trigger
   * - ``description``
     - string/null
     - Description of the Point Trigger
   * - ``category``
     - string
     - Category name
   * - ``isPublished``
     - boolean
     - Published state
   * - ``publishUp``
     - datetime/null
     - Date/time when the Point Trigger should be published
   * - ``publishDown``
     - datetime/null
     - Date/time the Point Trigger should be unpublished
   * - ``dateAdded``
     - datetime
     - Date/time the Point Trigger was created
   * - ``createdBy``
     - int
     - ID of the User that created the Point Trigger
   * - ``createdByUser``
     - string
     - Name of the User that created the Point Trigger
   * - ``dateModified``
     - datetime/null
     - Date/time the Point Trigger was last modified
   * - ``modifiedBy``
     - int
     - ID of the User that last modified the Point Trigger
   * - ``modifiedByUser``
     - string
     - Name of the User that last modified the Point Trigger
   * - ``points``
     - int
     - The minimum number of points before the trigger events are executed
   * - ``color``
     - string
     - Color hex to highlight the Contact with. This value does NOT include the pound sign (#)
   * - ``events``
     - array
     - Array of TriggerEvent entities for this Point Trigger. See below.

**Trigger Event Properties**

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the event
   * - ``type``
     - string
     - Event type
   * - ``name``
     - string
     - Name of the event
   * - ``description``
     - string
     - Description of the event
   * - ``order``
     - int
     - Event order
   * - ``properties``
     - array
     - Configured properties for the event

.. vale off

List Point Triggers
*******************

.. vale on

.. code-block:: php

   <?php
   // ...

   $triggers = $triggerApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);

.. code-block:: json

   {
       "total": 1,
       "triggers": [
           {
               "id": 1,
               "name": "Trigger test",
               "description": null,
               "category": null,
               "isPublished": true,
               "publishUp": null,
               "publishDown": null,
               "dateAdded": "2015-07-23T03:20:42-05:00",
               "createdBy": 1,
               "createdByUser": "Joe Smith",
               "dateModified": null,
               "modifiedBy": null,
               "modifiedByUser": null,
               "points": 10,
               "color": "ab5959",
               "events": {
                   "1": {
                       "id": 1,
                       "type": "email.send",
                       "name": "Send email",
                       "description": null,
                       "order": 1,
                       "properties": {
                           "email": 21
                       }
                   }
               }
           }
       ]
   }

.. vale off

**HTTP Request**

.. vale on

``GET /points/triggers``

.. vale off

**Query Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Description
   * - ``search``
     - String or search command to filter entities by.
   * - ``start``
     - Starting row for the entities returned. Defaults to 0.
   * - ``limit``
     - Limit number of entities to return. Defaults to the system configuration for pagination (30).
   * - ``orderBy``
     - Column to sort by. Can use any column listed in the response.
   * - ``orderByDir``
     - Sort direction: ``asc`` or ``desc``.
   * - ``publishedOnly``
     - Only return currently published entities.
   * - ``minimal``
     - Return only array of entities without additional lists in it.

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

See JSON code example.

.. vale off

**Properties**

.. vale on

Same as `Get Point Trigger <#get-point-trigger>`_.

.. vale off

Create Point Trigger
********************

.. vale on

.. code-block:: php

   <?php

   $data = array(
       'name' => 'test',
       'description' => 'created as a API test',
       'points' => 5,
       'color' => '4e5d9d',
       'trigger_existing_leads' => false,
       'events' => array(
           array(
               'name' => 'tag test event',
               'description' => 'created as a API test',
               'type' => 'lead.changetags',
               'order' => 1,
               'properties' => array(
                   'add_tags' => array('tag-a'),
                   'remove_tags' => array()
               )
           ),
           array(
               'name' => 'send email test event',
               'description' => 'created as a API test',
               'type' => 'email.send',
               'order' => 2,
               'properties' => array(
                   'email' => 1
               )
           )
       )
   );

   $trigger = $triggerApi->create($data);

Create a new Point Trigger.

.. vale off

**HTTP Request**

.. vale on

``POST /points/triggers/new``

.. vale off

**Post Parameters**

.. vale on

Same as `Get Point Trigger <#get-point-trigger>`_. Point Trigger events can be created/edited via the Point Trigger event arrays placed in the Point Trigger array.

.. vale off

**Response**

.. vale on

``Expected Response Code: 201``

.. vale off

**Properties**

.. vale on

Same as `Get Point Trigger <#get-point-trigger>`_.

.. vale off

Edit Point Trigger
******************

.. vale on

.. code-block:: php

   <?php

   $id   = 1;
   $data = array(
       'name' => 'test',
       'description' => 'created as a API test',
       'points' => 5,
       'color' => '4e5d9d',
       'trigger_existing_leads' => false,
       'events' => array(
           array(
               'name' => 'tag test event',
               'description' => 'created as a API test',
               'type' => 'lead.changetags',
               'order' => 1,
               'properties' => array(
                   'add_tags' => array('tag-a'),
                   'remove_tags' => array()
               )
           ),
           array(
               'name' => 'send email test event',
               'description' => 'created as a API test',
               'type' => 'email.send',
               'order' => 2,
               'properties' => array(
                   'email' => 1
               )
           )
       )
   );

   // Create new a Point Trigger if ID 1 isn't found?
   $createIfNotFound = true;

   $trigger = $triggerApi->edit($id, $data, $createIfNotFound);

Edit a new Point Trigger. Note that this supports PUT or PATCH depending on the desired behavior.

**PUT** creates a Point Trigger if the given ID doesn't exist and clears all the Point Trigger information, adds the information from the request. Point Trigger events are also deleted if not present in the request.
**PATCH** fails if the Point Trigger with the given ID doesn't exist and updates the Point Trigger field values with the values from the request.

.. vale off

**HTTP Request**

.. vale on

To edit a Point Trigger and return a 404 if the Point Trigger isn't found:

``PATCH /points/triggers/ID/edit``

To edit a Point Trigger and create a new one if the Point Trigger isn't found:

``PUT /points/triggers/ID/edit``

.. vale off

**Post Parameters**

.. vale on

Same as `Get Point Trigger <#get-point-trigger>`_. Point Trigger events can be created/edited via the Point Trigger event arrays placed in the Point Trigger array.

.. vale off

**Response**

.. vale on

If using ``PUT``, the expected response code is ``200`` if editing the Point Trigger or ``201`` if creating the Point Trigger.

If ``PATCH``, the expected response code is ``200``.

.. vale off

**Properties**

.. vale on

Same as `Get Point Trigger <#get-point-trigger>`_.

.. vale off

Delete Point Trigger
********************

.. vale on

.. code-block:: php

   <?php

   $trigger = $triggerApi->delete($id);

Delete a Point Trigger.

.. vale off

**HTTP Request**

.. vale on

``DELETE /points/triggers/ID/delete``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

.. vale off

**Properties**

.. vale on

Same as `Get Point Trigger <#get-point-trigger>`_.

.. vale off

Delete Point Trigger Events
***************************

.. vale on

The following examples show how to delete events with ID 56 and 59.

.. code-block:: php

   <?php

   $trigger = $triggerApi->deleteFields($triggerId, array(56, 59));

Delete Point Trigger events.

.. vale off

**HTTP Request**

.. vale on

``DELETE /points/triggers/ID/events/delete?events[]=56&events[]=59``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

.. vale off

**Properties**

.. vale on

Same as `Get Point Trigger <#get-point-trigger>`_.

.. vale off

Get Point Trigger Event Types
*****************************

.. vale on

.. code-block:: php

   <?php

   $point = $pointApi->getEventTypes();

Get an array of available Point Trigger Event Types.

.. vale off

**HTTP Request**

.. vale on

``GET /points/triggers/events/types``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

.. code-block:: json

   {
       "eventTypes": {
           "campaign.changecampaign": "Modify contact's campaigns",
           "lead.changelists": "Modify contact's segments",
           "lead.changetags": "Modify contact's tags",
           "plugin.leadpush": "Push contact to integration",
           "email.send": "Send an email"
       }
   }
