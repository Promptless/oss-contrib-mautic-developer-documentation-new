Pages
#####

.. vale off

.. note::

   The content for this page requires a major update. The legacy page contains outdated and potentially inaccurate information. You can still access it in the :xref:`legacy repository`.

   If you're interested in helping develop the new content for this page and others, consider joining the documentation efforts.

   Please read the :xref:`dev docs contributing guidelines` and :xref:`Contributing to Mautic’s documentation` to get started.

.. vale on

Use this endpoint to obtain details on Mautic's Landing Pages.

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
   $pageApi    = $api->newApi("pages", $auth, $apiUrl);

.. vale off

Get Page
********

.. vale on

.. code-block:: php

   <?php

   //...
   $page = $pageApi->get($id);

.. code-block:: json

   {
       "page": {
           "id": 3,
           "title": "Webinar Landing Page",
           "description": null,
           "isPublished": true,
           "publishUp": null,
           "publishDown": null,
           "dateAdded": "2015-07-15T15:06:02-05:00",
           "createdBy": 1,
           "createdByUser": "Joe Smith",
           "dateModified": "2015-07-20T13:11:56-05:00",
           "modifiedBy": 1,
           "modifiedByUser": "Joe Smith",
           "category": "Events",
           "language": "en",
           "template": "blank",
           "customHtml": "<html>Landing Page HTML</html>",
           "hits": 0,
           "uniqueHits": 0,
           "variantHits": 0,
           "revision": 1,
           "metaDescription": null,
           "redirectType": null,
           "redirectUrl": null,
           "translationChildren": [],
           "translationParent": null,
           "variantChildren": [],
           "variantParent": null,
           "variantSettings": [],
           "variantStartDate": null
       }
   }

Get an individual Landing Page by ID.

.. vale off

**HTTP Request**

.. vale on

``GET /pages/ID``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

See JSON code example.

**Page properties**

.. list-table::
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - int
     - ID of the Landing Page
   * - ``title``
     - string
     - Title of the Landing Page
   * - ``description``
     - string/null
     - Description of the Landing Page
   * - ``alias``
     - string
     - Used to generate the URL for the Landing Page
   * - ``isPublished``
     - boolean
     - Published state
   * - ``publishUp``
     - datetime/null
     - Date/time when the Landing Page should be published
   * - ``publishDown``
     - datetime/null
     - Date/time the Landing Page should be unpublished
   * - ``dateAdded``
     - datetime
     - Date/time the Landing Page was created
   * - ``createdBy``
     - int
     - ID of the User that created the Landing Page
   * - ``createdByUser``
     - string
     - Name of the User that created the Landing Page
   * - ``dateModified``
     - datetime/null
     - Date/time the Landing Page was last modified
   * - ``modifiedBy``
     - int
     - ID of the User that last modified the Landing Page
   * - ``modifiedByUser``
     - string
     - Name of the User that last modified the Landing Page
   * - ``language``
     - string
     - Language locale of the Landing Page
   * - ``template``
     - string
     - Template of the Landing Page
   * - ``customHtml``
     - string
     - Static HTML of the Landing Page
   * - ``hits``
     - int
     - Total Landing Page hit count
   * - ``uniqueHits``
     - int
     - Unique Landing Page hit count
   * - ``revision``
     - int
     - Landing Page revision
   * - ``metaDescription``
     - string
     - Meta description for the Landing Page's ``<head>``
   * - ``redirectType``
     - int
     - If unpublished, redirect with 301 or 302
   * - ``redirectUrl``
     - string
     - If unpublished, the URL to redirect to if ``redirectType`` is set
   * - ``translationChildren``
     - array
     - Array of Page entities for translations of this Landing Page
   * - ``translationParent``
     - object
     - The parent/main Landing Page if this is a translation
   * - ``variantHits``
     - int
     - Hit count since variantStartDate
   * - ``variantChildren``
     - array
     - Array of Page entities for variants of this Landing Page
   * - ``variantParent``
     - object
     - The parent/main Landing Page if this is a variant (A/B test)
   * - ``variantSettings``
     - array
     - The properties of the A/B test
   * - ``variantStartDate``
     - datetime/null
     - The date/time the A/B test began

.. vale off

List Pages
**********

.. vale on

.. code-block:: php

   <?php
   // ...

   $pages = $pageApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);

.. code-block:: json

   {
       "total": 1,
       "pages": [
           {
               "id": 3,
               "title": "Webinar Landing Page",
               "description": null,
               "isPublished": true,
               "publishUp": null,
               "publishDown": null,
               "dateAdded": "2015-07-15T15:06:02-05:00",
               "createdBy": 1,
               "createdByUser": "Joe Smith",
               "dateModified": "2015-07-20T13:11:56-05:00",
               "modifiedBy": 1,
               "modifiedByUser": "Joe Smith",
               "category": "Events",
               "language": "en",
               "template": "blank",
               "hits": 0,
               "uniqueHits": 0,
               "variantHits": 0,
               "revision": 1,
               "metaDescription": null,
               "redirectType": null,
               "redirectUrl": null,
               "translationChildren": [],
               "translationParent": null,
               "variantChildren": [],
               "variantParent": null,
               "variantSettings": [],
               "variantStartDate": null
           }
       ]
   }

Returns a list of Landing Pages available to the User.

.. vale off

**HTTP Request**

.. vale on

``GET /pages``

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

Same as `Get Page <#get-page>`_.

.. vale off

Create Page
***********

.. vale on

.. code-block:: php

   <?php

   $data = array(
       'title'       => 'Page A',
       'description' => 'This is my first Landing Page created via API.',
       'isPublished' => 1
   );

   $page = $pageApi->create($data);

Create a new Landing Page.

.. vale off

**HTTP Request**

.. vale on

``POST /pages/new``

.. vale off

**Post Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Description
   * - ``title``
     - Page title is the only required field
   * - ``alias``
     - Used to generate the URL for the Landing Page
   * - ``description``
     - A description of the Landing Page.
   * - ``isPublished``
     - A value of 0 or 1
   * - ``language``
     - Language locale of the Landing Page
   * - ``metaDescription``
     - Meta description for the Landing Page's ``<head>``
   * - ``redirectType``
     - If unpublished, redirect with 301 or 302
   * - ``redirectUrl``
     - If unpublished, the URL to redirect to if ``redirectType`` is set

.. vale off

**Response**

.. vale on

``Expected Response Code: 201``

.. vale off

**Properties**

.. vale on

Same as `Get Page <#get-page>`_.

.. vale off

Edit Page
*********

.. vale on

.. code-block:: php

   <?php

   $id   = 1;
   $data = array(
       'title'       => 'New Landing Page title',
       'isPublished' => 0
   );

   // Create a new Landing Page if ID 1 isn't found?
   $createIfNotFound = true;

   $page = $pageApi->edit($id, $data, $createIfNotFound);

Edit a Landing Page. Note that this supports PUT or PATCH depending on the desired behavior.

**PUT** creates a Landing Page if the given ID doesn't exist and clears all the Landing Page information, adds the information from the request.
**PATCH** fails if the Landing Page with the given ID doesn't exist and updates the Landing Page field values with the values from the request.

.. vale off

**HTTP Request**

.. vale on

To edit a Landing Page and return a 404 if the Landing Page isn't found:

``PATCH /pages/ID/edit``

To edit a Landing Page and create a new one if the Landing Page isn't found:

``PUT /pages/ID/edit``

.. vale off

**Post Parameters**

.. vale on

.. list-table::
   :header-rows: 1

   * - Name
     - Description
   * - ``title``
     - Page title is the only required field
   * - ``alias``
     - Name alias generated automatically if not set
   * - ``description``
     - A description of the Landing Page.
   * - ``isPublished``
     - A value of 0 or 1
   * - ``language``
     - Language locale of the Landing Page
   * - ``metaDescription``
     - Meta description for the Landing Page's ``<head>``
   * - ``redirectType``
     - If unpublished, redirect with 301 or 302
   * - ``redirectUrl``
     - If unpublished, the URL to redirect to if ``redirectType`` is set

.. vale off

**Response**

.. vale on

If using ``PUT``, the expected response code is ``200`` if the Landing Page was edited or ``201`` if created.

If ``PATCH``, the expected response code is ``200``.

.. vale off

**Properties**

.. vale on

Same as `Get Page <#get-page>`_.

.. vale off

Delete Page
***********

.. vale on

.. code-block:: php

   <?php

   $page = $pageApi->delete($id);

Delete a Landing Page.

.. vale off

**HTTP Request**

.. vale on

``DELETE /pages/ID/delete``

.. vale off

**Response**

.. vale on

``Expected Response Code: 200``

.. vale off

**Properties**

.. vale on

Same as `Get Page <#get-page>`_.
