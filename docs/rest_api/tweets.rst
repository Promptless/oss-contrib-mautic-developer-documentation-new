Tweets
######

Use this endpoint to manipulate and obtain details on Mautic's Tweets - reusable Tweet records that store the message text, language, and Category used by the Mautic Social bundle. The Tweets API was introduced in Mautic 2.8.0.

Using the Mautic API library
****************************

.. vale off

You can interact with this API using the :xref:`Mautic API Library` as below, or the various HTTP endpoints described in this document.

.. vale on

.. code-block:: php

   <?php
   use Mautic\MauticApi;
   use Mautic\Auth\ApiAuth;

   // ...
   $initAuth = new ApiAuth();
   $auth     = $initAuth->newAuth($settings);
   $apiUrl   = "https://example.com";
   $api      = new MauticApi();
   $tweetApi = $api->newApi("tweets", $auth, $apiUrl);

.. _get Tweet properties:

Tweet properties
****************

The following properties are returned when retrieving Tweets, and are accepted where applicable when creating or editing them.

.. list-table::
   :widths: 25 25 50
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``id``
     - integer
     - ID of the Tweet
   * - ``name``
     - string
     - Internal name of the Tweet
   * - ``text``
     - string
     - Message body of the Tweet. Supports dynamic tokens such as ``{twitter_handle}``
   * - ``isPublished``
     - boolean
     - Publication status of the Tweet
   * - ``publishUp``
     - datetime/null
     - Date and time when the Tweet should publish
   * - ``publishDown``
     - datetime/null
     - Date and time when the Tweet should unpublish
   * - ``dateAdded``
     - datetime
     - Date and time the Tweet was created
   * - ``createdBy``
     - integer
     - ID of the User who created the Tweet
   * - ``createdByUser``
     - string
     - Name of the User who created the Tweet
   * - ``dateModified``
     - datetime/null
     - Date and time the Tweet was last modified
   * - ``modifiedBy``
     - integer
     - ID of the User who last modified the Tweet
   * - ``modifiedByUser``
     - string
     - Name of the User who last modified the Tweet
   * - ``language``
     - string
     - Language locale of the Tweet
   * - ``category``
     - object/null
     - Category assigned to the Tweet
   * - ``description``
     - string/null
     - Internal description of the Tweet
   * - ``sentCount``
     - integer
     - Number of times the Tweet has been sent
   * - ``favoriteCount``
     - integer
     - Number of times the Tweet has been favorited
   * - ``retweetCount``
     - integer
     - Number of times the Tweet has been retweeted

.. vale off

Get Tweet
*********

.. vale on

Retrieves an individual Tweet by ID.

.. code-block:: php

   <?php

   //...
   $tweet = $tweetApi->get($id);

.. vale off

HTTP request
============

.. vale on

``GET /tweets/ID``

Response
========

* Returns ``200 OK`` when the request successfully retrieves the Tweet.

.. _get Tweet response:

.. code-block:: json

   {
       "tweet": {
           "isPublished": true,
           "dateAdded": "2026-02-03T17:51:58+01:00",
           "dateModified": "2026-03-28T11:03:03+02:00",
           "createdBy": 1,
           "createdByUser": "John Doe",
           "modifiedBy": 1,
           "modifiedByUser": "John Doe",
           "id": 1,
           "name": "Thank you tweet",
           "text": "Hi {twitter_handle}\n\nThanks for ...",
           "language": "en",
           "category": {
               "createdByUser": "John Doe",
               "modifiedByUser": null,
               "id": 185,
               "title": "Thank you tweets",
               "alias": "thank-you-tweets",
               "description": null,
               "color": "244bc9",
               "bundle": "global"
           },
           "sentCount": 3,
           "favoriteCount": 0,
           "retweetCount": 0,
           "description": "Used in the Product A campaign 1"
       }
   }

Properties
----------

Refer to :ref:`Tweet properties <get Tweet properties>`.

.. vale off

List Tweets
***********

.. vale on

Retrieves a list of Tweets.

.. code-block:: php

   <?php
   // ...

   $tweets = $tweetApi->getList($searchFilter, $start, $limit, $orderBy, $orderByDir, $publishedOnly, $minimal);

.. vale off

HTTP request
============

.. vale on

``GET /tweets``

Query parameters
----------------

.. list-table::
   :widths: 20 20 60
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``search``
     - string
     - String or search command to filter entities
   * - ``start``
     - integer
     - Starting row for the returned entities - defaults to 0
   * - ``limit``
     - integer
     - Maximum number of entities to return - defaults to the system pagination setting, typically 30
   * - ``orderBy``
     - string
     - Column to sort by. Any column in the response is valid.

       **Note**: convert ``camelCase`` properties to ``snake_case``. For example, ``dateAdded`` becomes ``date_added``
   * - ``orderByDir``
     - string
     - Sort direction - ``asc`` or ``desc``
   * - ``publishedOnly``
     - boolean
     - Returns only currently published entities
   * - ``minimal``
     - boolean
     - Returns only an array of entities without additional lists

Response
========

* Returns ``200 OK`` when the request successfully retrieves the Tweets list.

.. code-block:: json

   {
       "total": 1,
       "tweets": [
           {
               "isPublished": true,
               "dateAdded": "2026-02-03T17:51:58+01:00",
               "dateModified": "2026-03-28T11:03:03+02:00",
               "createdBy": 1,
               "createdByUser": "John Doe",
               "modifiedBy": 1,
               "modifiedByUser": "John Doe",
               "id": 1,
               "name": "Thank you tweet",
               "text": "Hi {twitter_handle}\n\nThanks for ...",
               "language": "en",
               "category": null,
               "favoriteCount": 0,
               "retweetCount": 0,
               "description": "Used in the Product A campaign 1"
           }
       ]
   }

Properties
----------

.. list-table::
   :widths: 25 25 50
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``total``
     - integer
     - Total number of Tweets matching the query
   * - ``tweets``
     - array
     - Array of Tweets

For the rest of the properties, refer to :ref:`Tweet properties <get Tweet properties>`.

.. vale off

Create Tweet
************

.. vale on

Creates a new Tweet.

.. code-block:: php

   <?php

   $data = array(
       'name'        => 'Tweet A',
       'text'        => 'This is my first Tweet created via API.',
       'isPublished' => 1,
   );

   $tweet = $tweetApi->create($data);

.. vale off

HTTP request
============

.. vale on

``POST /tweets/new``

.. _create Tweet POST parameters:

POST parameters
---------------

.. list-table::
   :widths: 25 25 50
   :header-rows: 1

   * - Name
     - Type
     - Description
   * - ``name``
     - string
     - **Required.**

       Internal name of the Tweet
   * - ``text``
     - string
     - **Required.**

       Message body of the Tweet
   * - ``isPublished``
     - boolean
     - Publication status. Defaults to ``1``
   * - ``publishUp``
     - datetime/null
     - Date and time when the Tweet should publish
   * - ``publishDown``
     - datetime/null
     - Date and time when the Tweet should unpublish
   * - ``language``
     - string
     - Language locale of the Tweet

Response
========

* Returns ``201 Created`` when the request successfully creates a Tweet.

The response is a JSON object similar to :ref:`Get Tweet <get Tweet response>`.

Properties
----------

Refer to :ref:`Tweet properties <get Tweet properties>`.

.. vale off

Edit Tweet
**********

.. vale on

Edits an existing Tweet.

This operation supports ``PUT`` or ``PATCH`` depending on the desired behavior:

* ``PUT``: **full replacement**. The request creates a new Tweet if the ID is missing. If the ID exists, the request clears all existing data and replaces it with the provided values.
* ``PATCH``: **partial update**. The request only updates field values based on the request data. The request fails when the Tweet ID doesn't exist.

.. code-block:: php

   <?php

   $id   = 1;
   $data = array(
       'name' => 'Tweet A',
       'text' => 'This is my first Tweet created via API.',
   );

   // Create a new Tweet if ID 1 isn't found
   $createIfNotFound = true;

   $tweet = $tweetApi->edit($id, $data, $createIfNotFound);

.. vale off

HTTP request
============

.. vale on

* ``PUT /tweets/ID/edit``: updates an existing Tweet or creates a new one when the ID doesn't exist.
* ``PATCH /tweets/ID/edit``: updates an existing Tweet. The request fails when the ID doesn't exist.

PUT/PATCH parameters
--------------------

Accepts the same parameters as those described in :ref:`Create Tweet <create Tweet POST parameters>`. All parameters are optional.

Response
========

* ``PUT``: returns ``200 OK`` when the request successfully updates the Tweet or ``201 Created`` when the request creates a Tweet.
* ``PATCH``: returns ``200 OK`` when the request successfully updates the Tweet or ``404 Not Found`` when the Tweet ID doesn't exist.

The response is a JSON object similar to :ref:`Get Tweet <get Tweet response>`.

Properties
----------

Refer to :ref:`Tweet properties <get Tweet properties>`.

.. vale off

Delete Tweet
************

.. vale on

Deletes a Tweet.

.. code-block:: php

   <?php

   $tweet = $tweetApi->delete($id);

.. vale off

HTTP request
============

.. vale on

``DELETE /tweets/ID/delete``

Response
========

* Returns ``200 OK`` when the request successfully deletes the Tweet.

The response is a JSON object containing the data of the deleted Tweet, similar to :ref:`Get Tweet <get Tweet response>`.

Properties
----------

Refer to :ref:`Tweet properties <get Tweet properties>`.
