Roles
#####

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Use this endpoint to obtain details on Mautic's Roles (administrators).

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
   $roleApi  = $api->newApi("roles", $auth, $apiUrl);

.. vale off

Get Role
********

.. vale on

.. code-block:: php

   <?php

   //...
   $role = $roleApi->get($id);

.. code-block:: json

   {
     "role":{
       "isPublished":true,
       "dateAdded":"2016-11-09T15:24:32+00:00",
       "createdBy":1,
       "createdByUser":"John Doe",
       "dateModified":null,
       "modifiedBy":null,
       "modifiedByUser":null,
       "id":13,
       "name":"API test role",
       "description":"created via AIP",
       "isAdmin":false,
       "rawPermissions":{
         "email:emails":[
           "viewown",
           "viewother"
         ]
       }
     }
   }

Get an individual Role by ID.

.. vale off

**HTTP Request**

.. vale on

``GET /roles/ID``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

See JSON code example.

**Role properties**

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Role
   * - ``dateAdded``
     - datetime
     - Date/time the Role was created
   * - ``createdBy``
     - int
     - ID of the User that created the Role
   * - ``createdByRole``
     - string
     - Name of the User that created the Role
   * - ``dateModified``
     - datetime/null
     - Date/time the Role was last modified
   * - ``modifiedBy``
     - int
     - ID of the User that last modified the Role
   * - ``modifiedByRole``
     - string
     - Name of the User that last modified the Role
   * - ``name``
     - string
     - Name of the Role
   * - ``description``
     - string
     - Description of the Role
   * - ``isAdmin``
     - boolean
     - Whether the Role has full access or only some
   * - ``rawPermissions``
     - array
     - Role permissions keyed by bundle:permission

.. vale off

List Contact Roles
******************

.. vale on

.. code-block:: php

   <?php

   //...
   $roles = $roleApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);

.. vale off

**HTTP Request**

.. vale on

``GET /roles``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

.. code-block:: json

   {
     "total":9,
     "roles":[
       {
         "isPublished":true,
         "dateAdded":"2016-08-01T11:51:32+00:00",
         "createdBy":1,
         "createdByUser":"John Doe",
         "dateModified":null,
         "modifiedBy":null,
         "modifiedByUser":null,
         "id":2,
         "name":"view email",
         "description":null,
         "isAdmin":false,
         "rawPermissions":{
           "email:emails":[
             "viewown",
             "viewother"
           ]
         }
       }
     ]
   }

**Role properties**

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Role
   * - ``dateAdded``
     - datetime
     - Date/time the Role was created
   * - ``createdBy``
     - int
     - ID of the User that created the Role
   * - ``createdByRole``
     - string
     - Name of the User that created the Role
   * - ``dateModified``
     - datetime/null
     - Date/time the Role was last modified
   * - ``modifiedBy``
     - int
     - ID of the User that last modified the Role
   * - ``modifiedByRole``
     - string
     - Name of the User that last modified the Role
   * - ``name``
     - string
     - Name of the Role
   * - ``description``
     - string
     - Description of the Role
   * - ``isAdmin``
     - boolean
     - Whether the Role has full access or only some
   * - ``rawPermissions``
     - array
     - Role permissions keyed by bundle:permission

.. vale off

Create Role
***********

.. vale on

.. code-block:: php

   <?php

   $data = array(
       'name' => 'API test role',
       'description' => 'created via AIP',
       'rawPermissions' => array (
           'email:emails' =>
           array (
               'viewown',
               'viewother',
           ),
       )
   );

   $role = $roleApi->create($data);

Create a new Role.

.. vale off

**HTTP Request**

.. vale on

``POST /roles/new``

.. vale off

**Post Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``name``
     - string
     - Name of the Role
   * - ``description``
     - string
     - Description of the Role
   * - ``isAdmin``
     - boolean
     - Whether the Role has full access or only some
   * - ``rawPermissions``
     - array
     - Role permissions keyed by bundle:permission

.. vale off

**Response**

.. vale on

``Expected Response Code: 201``

.. vale off

**Properties**

.. vale on

Same as `Get Role <#get-role>`_.

.. vale off

Edit Role
*********

.. vale on

.. code-block:: php

   <?php

   $id   = 1;
   $data = array(
       'name' => 'API test role',
       'description' => 'created via AIP',
       'rawPermissions' => array (
           'email:emails' =>
           array (
               'editown',
               'editother',
           ),
       )
   );

   // Create new a role of ID 1 is not found?
   $createIfNotFound = true;

   $role = $roleApi->edit($id, $data, $createIfNotFound);

Edit a new Role. Note that this supports PUT or PATCH depending on the desired behavior.

**PUT** creates a Role if the given ID doesn't exist and clears all the Role information, adds the information from the request.
**PATCH** fails if the Role with the given ID doesn't exist and updates the Role field values with the values from the request.

.. vale off

**HTTP Request**

.. vale on

To edit a Role and return a 404 if the Role isn't found:

``PATCH /roles/ID/edit``

To edit a Role and create a new one if the Role isn't found:

``PUT /roles/ID/edit``

.. vale off

**Post Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``name``
     - string
     - Name of the Role
   * - ``description``
     - string
     - Description of the Role
   * - ``isAdmin``
     - boolean
     - Whether the Role has full access or only some
   * - ``rawPermissions``
     - array
     - Role permissions keyed by bundle:permission

.. vale off

**Response**

.. vale on

If using ``PUT``, the expected response code is ``200`` if the Role was edited or ``201`` if created.

If ``PATCH``, the expected response code is ``200``.

.. vale off

**Properties**

.. vale on

Same as `Get Role <#get-role>`_.

.. vale off

Delete Role
***********

.. vale on

.. code-block:: php

   <?php

   $role = $roleApi->delete($id);

Delete a Role.

.. vale off

**HTTP Request**

.. vale on

``DELETE /roles/ID/delete``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

.. vale off

**Properties**

.. vale on

Same as `Get Role <#get-role>`_.
