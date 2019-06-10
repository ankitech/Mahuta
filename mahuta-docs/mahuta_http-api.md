Mahuta HTTP API
======

### Overview

##### Persistence

Represents the writting operations.


| Operation | Description | Method | URI |
| -------- | -------- | -------- | -------- |
| index_simple | Stored and index a text content | POST | /index |
| index_cid | Pin and index a CID | POST | /index/cid |
| index_file  | Store and index a file via HTTP Multipart | POST | /index/file ||

##### Query
Represents the read operations.

| Operation | Description | Method | URI |
| -------- | -------- | -------- | -------- |
| fetch | Get content | GET | /query/fetch/{hash} |
| search | Search content | POST | /query/search |

#### Delete

Represents the deletion operations.

| Operation | Description | Method | URI |
| -------- | -------- | -------- | -------- |
| delete_by_id | Unpin and deindex a file by ID | DELETE | /delete/id/{id} |
| delete_by_hash | Unpin and deindex a file by Hash | DELETE | /delete/hash/{hash} |


#### Configuration

Represents the configuration operations.

| Operation | Description | Method | URI |
| -------- | -------- | -------- | -------- |
| create_index | Create an index | POST | /config/index/{index} |
| get_indexes | Get all indexes | GET | /config/index |


### Details


#### [Persistence] Store and Index text

Store and Index a text

-   **URL** `/mahuta/index`
-   **Method:** `POST`
-   **Header:**  

| Key | Value |
| -------- | -------- |
| content-type | application/json |


-   **URL Params** `N/A`
-   **Data Params**

| Name | Type | Mandatory | Default | Description |
| -------- | -------- | -------- | -------- | -------- |
| content | String | yes |  | Content  |
| indexName | String | yes |  | Index name |
| indexDocId | String | no |  | Identifier of the document in the index. id null, autogenerated |
| contentType | String | no |  | Content type (mimetype) |
| indexFields | Object | no |  | Metadata can used to query the document |


```
{
  "content": "# Hello world,\n this is my first file stored on **IPFS**",
  "indexName": "articles",
  "indexDocId": "hello_world",
  "contentType": "text/markdown",
  "indexFields": {
      "title": "Hello world",
      "description": "Hello world this is my first file stored on IPFS",
      "author": "Gregoire Jeanmart",
      "votes": 10,
      "date_created": 1518700549,
      "tags": ["general"]
  }
}
```

-   **Sample Request:**

```
curl -X POST \
    'http://localhost:8040/mahuta/index' \
    -H 'content-type: application/json' \  
    -d '{"content":"# Hello world,\n this is my first file stored on **IPFS**","indexName":"articles","indexDocId":"hello_world","contentType":"text/markdown","index_fields":{"title":"Hello world","author":"Gregoire Jeanmart","votes":10,"date_created":1518700549,"tags":["general"]}}'
```

-   **Success Response:**

    -   **Code:** 200  
        **Content:**
```
{
    "status": "SUCCESS",
    "indexName": "articles",
    "id": "hello_world",
    "hash": "QmWPCRv8jBfr9sDjKuB5sxpVzXhMycZzwqxifrZZdQ6K9o"
}
```

---------------------------

#### [Persistence] Store and Index CID

Store and Index a text

-   **URL** `/mahuta/index/cid`
-   **Method:** `POST`
-   **Header:**  

| Key | Value |
| -------- | -------- |
| content-type | application/json |


-   **URL Params** `N/A`
-   **Data Params**

| Name | Type | Mandatory | Default | Description |
| -------- | -------- | -------- | -------- | -------- |
| cid | String | yes |  | Content Hash |
| indexName | String | yes |  | Index name |
| indexDocId | String | no |  | Identifier of the document in the index. id null, autogenerated |
| contentType | String | no |  | Content type (mimetype) |
| indexFields | Object | no |  | Metadata can used to query the document |


```
{
  "cid": "QmWPCRv8jBfr9sDjKuB5sxpVzXhMycZzwqxifrZZdQ6K9o",
  "indexName": "articles",
  "indexDocId": "hello_world",
  "contentType": "text/markdown",
  "indexFields": {
      "title": "Hello world",
      "description": "Hello world this is my first file stored on IPFS",
      "author": "Gregoire Jeanmart",
      "votes": 10,
      "date_created": 1518700549,
      "tags": ["general"]
  }
}
```

-   **Sample Request:**

```
curl -X POST \
    'http://localhost:8040/mahuta/index/cid' \
    -H 'content-type: application/json' \  
    -d '{"cid":"QmWPCRv8jBfr9sDjKuB5sxpVzXhMycZzwqxifrZZdQ6K9o","indexName":"articles","indexDocId":"hello_world","contentType":"text/markdown","index_fields":{"title":"Hello world","author":"Gregoire Jeanmart","votes":10,"date_created":1518700549,"tags":["general"]}}'
```

-   **Success Response:**

    -   **Code:** 200  
        **Content:**
```
{
    "status": "SUCCESS",
    "indexName": "articles",
    "id": "hello_world",
    "hash": "QmWPCRv8jBfr9sDjKuB5sxpVzXhMycZzwqxifrZZdQ6K9o"
}
```

---------------------------

#### [Persistencd] Store and Index file

Store content in IPFS and index it into the search engine

-   **URL** `/mahuta/index/file`
-   **Method:** `POST`
-   **Header:** `N/A`
-   **URL Params** `N/A`
-   **Data Params**


    -   `file: [content]`
    -   `request: `

| Name | Type | Mandatory | Default | Description |
| -------- | -------- | -------- | -------- | -------- |
| indexName | String | yes |  | Index name |
| indexDocId | String | no |  | Identifier of the document in the index. id null, autogenerated |
| contentType | String | no |  | Content type (mimetype) |
| indexFields | Object | no |  | Metadata can used to query the document |


```
{
  "indexName": "articles",
  "indexDocId": "hello_world",
  "contentType": "text/markdown",
  "indexFields": {
      "title": "Hello world",
      "description": "Hello world this is my first file stored on IPFS",
      "author": "Gregoire Jeanmart",
      "votes": 10,
      "date_created": 1518700549,
      "tags": ["general"]
  }
}
```

-   **Sample Request:**

```
curl -X POST \
  http://localhost:8040/mahuta/index/file \
  -H 'content-type: multipart/form-data; boundary=----WebKitFormBoundary7MA4YWxkTrZu0gW' \
  -F file=@/home/gjeanmart/hello.pdf \
  -F 'request='{"indexName":"articles","indexDocId":"hello_world","contentType":"text/markdown","index_fields":{"title":"Hello world","author":"Gregoire Jeanmart","votes":10,"date_created":1518700549,"tags":["general"]}}'
```

-   **Success Response:**

    -   **Code:** 200  
        **Content:**
```
{
    "status": "SUCCESS",
    "indexName": "articles",
    "id": "hello_world",
    "hash": "QmWPCRv8jBfr9sDjKuB5sxpVzXhMycZzwqxifrZZdQ6K9o"
}
```

---------------------------

#### [Query] Get content

Get content on IPFS by hash

-   **URL** `http://localhost:8040/mahuta/query/fetch/{hash}`
-   **Method:** `GET`
-   **Header:**  `N/A`
-   **URL Params** `N/A`

-   **Sample Request:**

```
$ curl \
    'http://localhost:8040/mahuta/query/fetch/QmWPCRv8jBfr9sDjKuB5sxpVzXhMycZzwqxifrZZdQ6K9o' \
    -o hello_doc.pdf
```

-   **Success Response:**

    -   **Code:** 200  
        **Content:** (file)

---------------------------

#### [Query] Search contents

Search content accross an index using a dedicated query language

-   **URL** `http://localhost:8040/mahuta/query/search`
-   **Method:** `GET` or `POST`
-   **Header:**  

| Key | Value |
| -------- | -------- |
| content-type | application/json |

-   **URL Params**

| Name | Type | Mandatory | Default | Description |
| -------- | -------- | -------- | -------- | -------- |
| index | String | no |  | Index to search (if null: all indexes) |
| pageNo | Int | no | 0 | Page Number |
| pageSize | Int | no | 20 | Page Size / Limit |
| sort | String | no |  | Sorting attribute |
| dir | ASC/DESC | no | ASC | Sorting direction |
| query | String | no |  | Query |


-   **Data Params**

The `search` operation allows to run a multi-criteria search against an index. The body combines a list of filters :

| Name | Type | Description |
| -------- | -------- | -------- |
| name | String | Index field to perform the search |
| names | String[] | Index fields to perform the search |
| operation | See below | Operation to run against the index field |
| value | Any | Value to compare with |



| Operation | Description |
| -------- | -------- |
| FULL_TEXT | Full text search |
| EQUALS | Equals |
| NOT_EQUALS | Not equals |
| CONTAINS | Contains the word/phrase |
| IN | in the following list |
| GT | Greater than |
| GTE | Greater than or Equals |
| LT | Less than  |
| LTE | Less than or Equals |


```
{
  "query": [
    {
      "name": "title",
      "operation": "CONTAINS",
      "value": "Hello"
    },
    {
      "names": ["description", "title"],
      "operation": "FULL_TEXT",
      "value": "IPFS"
    },
    {
      "name": "votes",
      "operation": "LT",
      "value": "5"
    }
  ]
}
```

-   **Sample Request:**

```
curl -X POST \
    'http://localhost:8040/mahuta/query/search?index=articles' \
    -H 'content-type: application/json' \  
    -d '{"query":[{"name":"title","operation":"CONTAINS","value":"Hello"},{"name":"author","operation":"EQUALS","value":"Gregoire Jeanmart"},{"name":"votes","operation":"LT","value":"5"}]}'
```

-   **Success Response:**

    -   **Code:** 200  
        **Content:**

```
{
  "elements": [
    {
        "metadata": {
          "indexName": "articles",
          "indexDocId": "hello_world",
          "contentId": "QmWPCRv8jBfr9sDjKuB5sxpVzXhMycZzwqxifrZZdQ6K9o",
          "contentType": "application/pdf",
          "indexFields": {
              "title": "Hello world",
              "description": "Hello world this is my first file stored on IPFS",
              "author": "Gregoire Jeanmart",
              "votes": 10,
              "date_created": 1518700549,
              "tags": ["general"]
          }
        },
        "payload": "# Hello world,\n this is my first file stored on **IPFS**"
    }
  ]
}
],,
"totalElements": 4,
"totalPages": 1
}
```