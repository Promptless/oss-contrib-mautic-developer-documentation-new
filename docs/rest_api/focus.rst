Focus
#####

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Use this endpoint to obtain details on Mautic's Focus Items.

**Using Mautic's API Library**

You can interact with this API through the :xref:`Mautic API Library` as follows, or use the various http endpoints as described in this document.

.. code-block:: php

   <?php
   use Mautic\MauticApi;
   use Mautic\Auth\ApiAuth;

   // ...
   $initAuth = new ApiAuth();
   $auth     = $initAuth->newAuth($settings);
   $apiUrl   = "https://example.com";
   $api      = new MauticApi();
   $focusApi = $api->newApi("focus", $auth, $apiUrl);

.. vale off

Get Focus Item
**************

.. vale on

.. code-block:: php

   <?php

   //...
   $focus = $focusApi->get($id);

.. code-block:: json

   {
     "focus": {
       "isPublished": true,
       "dateAdded": "2016-06-20T11:26:51+00:00",
       "createdBy": 1,
       "createdByUser": "John Doe",
       "dateModified": "2016-08-08T16:36:27+00:00",
       "modifiedBy": 1,
       "modifiedByUser": "John Doe",
       "category": null,
       "publishUp": null,
       "publishDown": null,
       "id": 1,
       "name": "Focus Bar",
       "type": "notice",
       "website": "",
       "htmlMode": "0",
       "html": "<div><strong style=\"color:red\">your html code</strong></div>",
       "css": ".mf-bar-collapser {border-radius: 0 !important}",
       "properties": {
         "bar": {
           "allow_hide": 1,
           "sticky": 1,
           "size": "large",
           "placement": "top"
         },
         "modal": {
           "placement": "top"
         },
         "notification": {
           "placement": "top"
         },
         "colors": {
           "primary": "27184e"
         },
         "content": {
           "headline": "",
           "tagline": "",
           "link_text": "",
           "link_url": "",
           "font": "Arial, Helvetica, sans-serif"
         },
         "animate": "1",
         "link_activation": "1",
         "when": "immediately",
         "frequency": "everypage",
         "stop_after_conversion": "1",
         "form": ""
       }
     }
   }

Get an individual Focus Item by ID.

.. vale off

**HTTP Request**

.. vale on

``GET /focus/ID``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

See JSON code example.

**Focus Item properties**

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Focus Item
   * - ``name``
     - string
     - Name of the Focus Item
   * - ``description``
     - string/null
     - Description of the Focus Item
   * - ``isPublished``
     - boolean
     - Published state
   * - ``publishUp``
     - datetime/null
     - Date/time when the Focus Item should be published
   * - ``publishDown``
     - datetime/null
     - Date/time the Focus Item should be unpublished
   * - ``dateAdded``
     - datetime
     - Date/time Focus Item was created
   * - ``createdBy``
     - int
     - ID of the user that created the Focus Item
   * - ``createdByUser``
     - string
     - Name of the user that created the Focus Item
   * - ``dateModified``
     - datetime/null
     - Date/time Focus Item was last modified
   * - ``modifiedBy``
     - int
     - ID of the user that last modified the Focus Item
   * - ``modifiedByUser``
     - string
     - Name of the user that last modified the Focus Item

.. vale off

List Focus Items
****************

.. vale on

.. code-block:: php

   <?php
   // ...

   $focus = $focusApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);

.. code-block:: json

   {
     "total": 30,
     "focus": [
       {
         "isPublished": true,
         "dateAdded": "2016-06-20T11:26:51+00:00",
         "createdBy": 1,
         "createdByUser": "John Doe",
         "dateModified": "2016-08-08T16:36:27+00:00",
         "modifiedBy": 1,
         "modifiedByUser": "John Doe",
         "category": null,
         "publishUp": null,
         "publishDown": null,
         "id": 1,
         "name": "Focus Bar",
         "type": "notice",
         "website": "",
         "htmlMode": "0",
         "html": "<div><strong style=\"color:red\">your html code</strong></div>",
         "css": ".mf-bar-collapser {border-radius: 0 !important}",
         "properties": {
           "bar": {
             "allow_hide": 1,
             "sticky": 1,
             "size": "large",
             "placement": "top"
           },
           "modal": {
             "placement": "top"
           },
           "notification": {
             "placement": "top"
           },
           "colors": {
             "primary": "27184e"
           },
           "content": {
             "headline": "",
             "tagline": "",
             "link_text": "",
             "link_url": "",
             "font": "Arial, Helvetica, sans-serif"
           },
           "animate": "1",
           "link_activation": "1",
           "when": "immediately",
           "frequency": "everypage",
           "stop_after_conversion": "1",
           "form": ""
         }
       }
     ]
   }

.. vale off

**HTTP Request**

.. vale on

``GET /focus``

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

Same as `Get Focus Item <#get-focus-item>`_.

.. vale off

Create Focus Item
*****************

.. vale on

.. code-block:: php

   <?php

   $data = array(
       'name'        => 'Focus Item',
       'isPublished' => 1
   );

   $focus = $focusApi->create($data);

Create a new Focus Item.

.. vale off

**HTTP Request**

.. vale on

``POST /focus/new``

.. vale off

**Post Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Focus Item
   * - ``name``
     - string
     - Name of the Focus Item
   * - ``description``
     - string/null
     - Description of the Focus Item
   * - ``isPublished``
     - boolean
     - Published state
   * - ``publishUp``
     - datetime/null
     - Date/time when the Focus Item should be published
   * - ``publishDown``
     - datetime/null
     - Date/time the Focus Item should be unpublished
   * - ``dateAdded``
     - datetime
     - Date/time Focus Item was created
   * - ``createdBy``
     - int
     - ID of the user that created the Focus Item
   * - ``createdByUser``
     - string
     - Name of the user that created the Focus Item
   * - ``dateModified``
     - datetime/null
     - Date/time Focus Item was last modified
   * - ``modifiedBy``
     - int
     - ID of the user that last modified the Focus Item
   * - ``modifiedByUser``
     - string
     - Name of the user that last modified the Focus Item

.. vale off

**Response**

.. vale on

``Expected Response Code: 201``

.. vale off

**Properties**

.. vale on

Same as `Get Focus Item <#get-focus-item>`_.

.. vale off

Edit Focus Item
***************

.. vale on

.. code-block:: php

   <?php

   $id   = 1;
   $data = array(
       'name'        => 'New focus name',
       'isPublished' => 0
   );

   // Create new a focus of ID 1 is not found?
   $createIfNotFound = true;

   $focus = $focusApi->edit($id, $data, $createIfNotFound);

Edit a new Focus Item. Note that this supports PUT or PATCH depending on the desired behavior.

**PUT** creates a Focus Item if the given ID doesn't exist and clears all the Focus Item information, adds the information from the request.
**PATCH** fails if the Focus Item with the given ID doesn't exist and updates the Focus Item field values with the values from the request.

.. vale off

**HTTP Request**

.. vale on

To edit a Focus Item and return a 404 if the Focus Item isn't found:

``PATCH /focus/ID/edit``

To edit a Focus Item and create a new one if the Focus Item isn't found:

``PUT /focus/ID/edit``

.. vale off

**Post Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Focus Item
   * - ``name``
     - string
     - Name of the Focus Item
   * - ``description``
     - string/null
     - Description of the Focus Item
   * - ``isPublished``
     - boolean
     - Published state
   * - ``publishUp``
     - datetime/null
     - Date/time when the Focus Item should be published
   * - ``publishDown``
     - datetime/null
     - Date/time the Focus Item should be unpublished
   * - ``dateAdded``
     - datetime
     - Date/time Focus Item was created
   * - ``createdBy``
     - int
     - ID of the user that created the Focus Item
   * - ``createdByUser``
     - string
     - Name of the user that created the Focus Item
   * - ``dateModified``
     - datetime/null
     - Date/time Focus Item was last modified
   * - ``modifiedBy``
     - int
     - ID of the user that last modified the Focus Item
   * - ``modifiedByUser``
     - string
     - Name of the user that last modified the Focus Item

.. vale off

**Response**

.. vale on

If using ``PUT``, the expected response code is ``200`` if editing the Focus Item or ``201`` if creating the Focus Item.

If ``PATCH``, the expected response code is ``200``.

.. vale off

**Properties**

.. vale on

Same as `Get Focus Item <#get-focus-item>`_.

.. vale off

Delete Focus Item
*****************

.. vale on

.. code-block:: php

   <?php

   $focus = $focusApi->delete($id);

Delete a Focus Item.

.. vale off

**HTTP Request**

.. vale on

``DELETE /focus/ID/delete``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

.. vale off

**Properties**

.. vale on

Same as `Get Focus Item <#get-focus-item>`_.
