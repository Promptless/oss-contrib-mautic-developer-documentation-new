Notes
#####

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Use this endpoint to obtain details on Mautic's Contact Notes.

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
   $noteApi  = $api->newApi("notes", $auth, $apiUrl);

.. vale off

Get Note
********

.. vale on

.. code-block:: php

   <?php

   //...
   $note = $noteApi->get($id);

.. code-block:: json

   {
     "note":{
       "id":39,
       "text":"Contact note created via API request",
       "type":"general",
       "dateTime":null,
       "lead":{
         "id":1405,
         "points":0,
         "color":null,
         "fields":{
           "core":{
             "firstname":{
               "id":"2",
               "label":"First Name",
               "alias":"firstname",
               "type":"text",
               "group":"core",
               "field_order":"42",
               "object":"lead",
               "value":"Note API test"
             },
             "lastname":{
               "id":"3",
               "label":"Last Name",
               "alias":"lastname",
               "type":"text",
               "group":"core",
               "field_order":"44",
               "object":"lead",
               "value":null
             }
           }
         }
       }
     }
   }

Get an individual Note by ID.

.. vale off

**HTTP Request**

.. vale on

``GET /notes/ID``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

See JSON code example.

**Note Properties**

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Note
   * - ``lead``
     - array
     - Data of the Contact
   * - ``text``
     - string
     - Note text
   * - ``type``
     - string
     - Note type
   * - ``datetime``
     - datetime
     - Date and time related to the Note.

.. vale off

List Contact Notes
******************

.. vale on

.. code-block:: php

   <?php

   //...
   $notes = $noteApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);

.. code-block:: json

   {
     "total":24,
     "notes":[
       {
         "id":1,
         "text":"A test note",
         "type":"general",
         "dateTime":"2016-06-14T18:07:00+00:00",
         "lead":{
           "id":1,
           "points":0,
           "color":null,
           "fields":[]
         }
       }
     ]
   }

.. vale off

**HTTP Request**

.. vale on

``GET /notes``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

See JSON code example.

**Note Properties**

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Note
   * - ``lead``
     - array
     - Data of the Contact
   * - ``text``
     - string
     - Note text
   * - ``type``
     - string
     - Note type
   * - ``datetime``
     - datetime
     - Date and time related to the Note.

.. vale off

Create Note
***********

.. vale on

.. code-block:: php

   <?php

   $contactID = 1;

   $data = array(
       'lead' => $contactID,
       'text' => 'Note A',
       'type' => 'general',
   );

   $note = $noteApi->create($data);

Create a new Note.

.. vale off

**HTTP Request**

.. vale on

``POST /notes/new``

.. vale off

**Post Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``text``
     - string
     - Note text
   * - ``type``
     - string
     - Note type
   * - ``datetime``
     - datetime
     - Date and time related to the Note.

.. vale off

**Response**

.. vale on

``Expected Response Code: 201``

.. vale off

**Properties**

.. vale on

Same as `Get Note`.

.. vale off

Edit Note
*********

.. vale on

.. code-block:: php

   <?php

   $id   = 1;
   $data = array(
       'text' => 'Note B',
       'type' => 'general',
   );

   // Create new a note of ID 1 is not found?
   $createIfNotFound = true;

   $note = $noteApi->edit($id, $data, $createIfNotFound);

Edit a Note. Note that this supports PUT or PATCH depending on the desired behavior.

**PUT** creates a Note if the given ID doesn't exist and clears all the Note information, adds the information from the request.

**PATCH** fails if the Note with the given ID doesn't exist and updates the Note field values with the values from the request.

.. vale off

**HTTP Request**

.. vale on

To edit a Note and return a 404 if the Note isn't found:

``PATCH /notes/ID/edit``

To edit a Note and create a new one if the Note isn't found:

``PUT /notes/ID/edit``

.. vale off

**Post Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``text``
     - string
     - Note text
   * - ``type``
     - string
     - Note type
   * - ``datetime``
     - datetime
     - Date and time related to the Note.

.. vale off

**Response**

.. vale on

If ``PUT``, the expected response code is ``200`` when Mautic edits an existing Note or ``201`` when it creates a new one.

If ``PATCH``, the expected response code is ``200``.

.. vale off

**Properties**

.. vale on

Same as `Get Note`.

.. vale off

Delete Note
***********

.. vale on

.. code-block:: php

   <?php

   $note = $noteApi->delete($id);

Delete a Note.

.. vale off

**HTTP Request**

.. vale on

``DELETE /notes/ID/delete``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

.. vale off

**Properties**

.. vale on

Same as `Get Note`.
