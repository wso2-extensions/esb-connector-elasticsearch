# Working with Documents in Elasticsearch

[[Overview]](#overview)  [[Operation details]](#operation-details)  [[Sample configuration]](#sample-configuration)

### Overview 

The following operations allow you to work with documents. Click an operation name to see details on how to use it.
For a sample proxy service that illustrates how to work with documents, see [Sample configuration](#sample-configuration).

| Operation        | Description |
| ------------- |-------------|
| [createDocumentWithIndex](#creating-or-updating-a-document-in-a-specified-index)    | Creates or updates a document in a specified index. |
| [createAutomaticIndex](#creating-a-given-index)      | Creates a given index. |
| [createAutomaticId](#creating-an-id-automatically)    | Creates an ID automatically. |
| [routeDocument](#routing-the-document)    | Routes the document.    |
| [indexChildDocument](#setting-the-child-document-index)    | Sets the child document index. |
| [getDocument](#retrieving-a-typed-json-document)      | Retrieves a typed JSON document from an index based on a specified ID.  |
| [deleteDocument](#deleting-a-typed-json-document )    | Deletes a typed JSON document from an index based on a specified ID. |
| [listDocuments](#retrieving-multiple-documents )    | Retrieves multiple documents based on the specified index, type, and ID.    |
| [updateDocument](#updating-a-document )    | Updates a document based on a script provided. |
| [performBulkOperations](#performing-multiple-index/delete-operations)      | Performs multiple index/delete operations in a single API call. |

### Operation details

This section provides further details on the operations related to documents.

#### Creating or updating a document in a specified index

The createDocumentWithIndex operation creates or updates a document in a specified index.

**createDocumentWithIndex**
```xml
<elasticsearch.createDocumentWithIndex>
    <indexName>{$ctx:indexName}</indexName>
    <type>{$ctx:type}</type>
    <id>{$ctx:id}</id>
    <user>{$ctx:user}</user>
    <postDate>{$ctx:postDate}</postDate>
    <message>{$ctx:message}</message>
    <version>{$ctx:version}</version>
    <versionType>{$ctx:versionType}</versionType>
    <operationType>{$ctx:operationType}</operationType>
    <timeStamp>{$ctx:timeStamp}</timeStamp>
    <timeToLive>{$ctx:timeToLive}</timeToLive>
    <timeout>{$ctx:timeout}</timeout>
    <refresh>{$ctx:refresh}</refresh>
    <consistency>{$ctx:consistency}</consistency>
</elasticsearch.createDocumentWithIndex>
```

**Properties**
* indexName: The name of the index.
* type: The type of the document.
* id: The identifier of the document that is created/updated.
* user: The name of the user performing the operation.
* postDate: The date that the index should be inserted. If a date is not explicitly specified, this will be the default processing date.
* message: The message associated with document.
* version: The version number of the document.
* versionType: The specific version type of the document. Possible values are: internal, external and external_gte.
* operationType: The operation type to be used when indexing documents.
* timeStamp: The timestamp that the document should be indexed. If the timestamp value is not provided externally or in the source, the timestamp will be automatically set to the timestamp that the document was processed by the indexing chain.
* timeToLive: The duration after which a document should expire and be deleted automatically.  <br/>
  **Note :**
  A document can be indexed with a ttl (time to live) associated with it. Expired documents will be deleted automatically. The expiration that is set for a document with a provided ttl is relative to the timestamp of the document, which means that it can be based on the time of indexing or on any time provided.
* timeout: A timeout value for the operation. This is the duration that the index operation waits for the primary shard to become available, at a time that the primary shard assigned to perform the index operation is not available.
* refresh: Set this based on when you want changes made by this operation to be made visible to search results. Set this to true if you want to refresh the shard (not the whole index) immediately after the operation occurs, so that the document appears in search results immediately.  <br/>
  **Note :**
  Setting this to true should only be done after thoroughly verifying that it does not lead to poor performance, both from an indexing and a search standpoint.
* consistency: An appropriate value to prevent the operation from starting when the number of active shards within that partition (replication group) are not sufficient. This prevent writes from taking place on the wrong side of a network partition. Possible values are: one, quorum, and all.

**Sample request**

Following is a sample request that can be handled by the createDocumentWithIndex operation.

```json
{
    "apiUrl": "http://localhost:9200",
    "pretty": "true",
    "format": "yaml",
    "human": "true",
    "filterPath": "_index,_type",
    "flatSettings": "true",
    "callback": "true",
    "case": "camelCase",
    "indexName": "twitter",
    "type": "tweet",
    "id": "402",
    "user": "kimchy",
    "postDate": "2009-11-15T14:12:12",
    "message": "trying out Elasticsearch",
    "version": "2",
    "versionType": "external",
    "operationType": "create",
    "timeStamp": "2009-11-15T14:12:12",
    "timeToLive": "86400000",
    "timeout": "5m",
    "refresh": "true",
    "consistency": "quorum"
}
```

**Sample response**

Given below is a sample response for the createDocumentWithIndex operation.

```json
{
   "_index":"1537433624954index_name",
   "_type":"tweet",
   "_id":"documentIdOptional_1537433624954",
   "_version":23,
   "result":"created",
   "_shards":{
      "total":2,
      "successful":1,
      "failed":0
   },
   "created":true
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html#docs-index_

#### Creating a given index

The createAutomaticIndex operation creates a given index.

**createAutomaticIndex**
```xml
<elasticsearch.createAutomaticIndex>
    <settings>{$ctx:settings}</settings>
    <indexName>{$ctx:indexName}</indexName>
</elasticsearch.createAutomaticIndex>
```

**Properties**
* settings: The settings you need to have for the index that is created.
* indexName: The name of the index to be created.

**Sample request**

Following is a sample request that can be handled by the createAutomaticIndex operation.

```json
{
   "apiUrl":"http://172.22.217.169:9200/",
   "pretty":"true",
   "format":"yaml",
   "human":"true",
   "filterPath":"_index,_type",
   "flatSettings":"true",
   "callback":"true",
   "case":"camelCase",
   "settings":{"index": {"mapping.allow_type_wrapper": true}},
   "indexName":"index"
}
```

**Sample response**

Given below is a sample response for the createAutomaticIndex operation.

```json
{
   "acknowledged":true,
   "shards_acknowledged":true,
   "index":"1537433624954index_man"
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html#index-creation

#### Creating an ID automatically

The createAutomaticId operation creates an ID automatically when indexing is done without specifying the ID.

**createAutomaticId**
```xml
<elasticsearch.createAutomaticId>
    <message>{$ctx:message}</message>
    <timeStamp>{$ctx:timeStamp}</timeStamp>
    <postDate>{$ctx:postDate}</postDate>
    <consistency>{$ctx:consistency}</consistency>
    <indexName>{$ctx:indexName}</indexName>
    <type>{$ctx:type}</type>
    <refresh>{$ctx:refresh}</refresh>
    <user>{$ctx:user}</user>
    <timeout>{$ctx:timeout}</timeout>
    <timeToLive>{$ctx:timeToLive}</timeToLive>
</elasticsearch.createAutomaticId>
```

**Properties**
* message: The message associated with the document.
* timeStamp: The timestamp that the document should be indexed. If the timestamp value is not provided externally or in the source, the timestamp will be automatically set to the timestamp that the document was processed by the indexing chain.
* postDate: The date that the index should be inserted. If a date is not explicitly specified, this will be the default processing date.
* consistency: An appropriate value to prevent the operation from starting when the number of active shards within that partition (replication group) are not sufficient. This prevent writes from taking place on the wrong side of a network partition. Possible values are: one, quorum, and all.
* indexName: The name of the index where the document is indexed.
* type: The type of the document.
* refresh: Set this based on when you want changes made by this operation to be made visible to search results. Set this to true if you want to refresh the shard (not the whole index) immediately after the operation occurs, so that the document appears in search results immediately. <br/>
  **Note:**
  Setting this to true should only be done after thoroughly verifying that it does not lead to poor performance, both from an indexing and a search standpoint.
* user: The name of the user performing the operation.
* timeout: A timeout value for the operation. This is the duration that the index operation waits for the primary shard to become available, at a time that the primary shard assigned to perform the index operation is not available.
* timeToLive: The duration after which a document should expire and be deleted automatically.  <br/>
  **Note:**
  A document can be indexed with a ttl (time to live) associated with it. Expired documents will be deleted automatically. The expiration that is set for a document with a provided ttl is relative to the timestamp of the document, which means that it can be based on the time of indexing or on any time provided.

**Sample request**

Following is a sample request that can be handled by the createAutomaticId operation.

```json
{
    "apiUrl": "http://localhost:9200",
    "pretty": "true",
    "human": "true",
    "filterPath": "_index,_type",
    "flatSettings": "true",
    "callback": "true",
    "case": "camelCase",
    "message": "trying out Elasticsearch",
    "timeStamp": "2009-11-15T14:12:12",
    "postDate": "2009-11-15T14:12:12",
    "consistency": "quorum",
    "indexName": "twitter",
    "type": "tweet",
    "refresh": "true"
    "user": "kimchy",
    "timeout": "5m",
    "timeToLive": "86400000"   
}
```

**Sample response**

Given below is a sample response for the createAutomaticId operation.

```json
{
   "_index":"1537433624954index_name",
   "_type":"tweet",
   "_id":"AWX2L2uNKTdgaaIiclE2",
   "_version":1,
   "result":"created",
   "_shards":{
      "total":2,
      "successful":1,
      "failed":0
   },
   "created":true
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html#_automatic_id_generation

#### Routing the document

The routeDocument operation routes a document to a shard base.

**routeDocument**
```xml
<elasticsearch.routeDocument>
    <indexName>{$ctx:indexName}</indexName>
    <type>{$ctx:type}</type>
    <user>{$ctx:user}</user>
    <postDate>{$ctx:postDate}</postDate>
    <message>{$ctx:message}</message>
    <routingName>{$ctx:routingName}</routingName>
    <timeStamp>{$ctx:timeStamp}</timeStamp>
    <timeToLive>{$ctx:timeToLive}</timeToLive>
    <timeout>{$ctx:timeout}</timeout>
    <refresh>{$ctx:refresh}</refresh>
    <consistency>{$ctx:consistency}</consistency>
</elasticsearch.routeDocument>
```
**Properties**
* indexName: The name of the index where the document should be indexed.
* type: The type of the document.
* user: The name of the user performing the operation.
* postDate: The date that the index should be inserted. If a date is not explicitly specified, this will be the default processing date.
* message: The message associated with document.
* routingName: The name of the shard base to which the document should be routed.
* timeStamp: The timestamp that the document should be indexed. If the timestamp value is not provided externally or in the source, the timestamp will be automatically set to the timestamp that the document was processed by the indexing chain.
* timeToLive: The duration after which a document should expire and be deleted automatically.  <br/>
  **Note:**
  A document can be indexed with a ttl (time to live) associated with it. Expired documents will be deleted automatically. The expiration that is set for a document with a provided ttl is relative to the timestamp of the document, which means that it can be based on the time of indexing or on any time provided.
* timeout: A timeout value for the operation. This is the duration that the index operation waits for the primary shard to become available, at a time that the primary shard assigned to perform the index operation is not available.
* refresh: Set this based on when you want changes made by this operation to be made visible to search results. Set this to true if you want to refresh the shard (not the whole index) immediately after the operation occurs, so that the document appears in search results immediately.  <br/>
  **Note:**
  Setting this to true should only be done after thoroughly verifying that it does not lead to poor performance, both from an indexing and a search standpoint.
* consistency: An appropriate value to prevent the operation from starting when the number of active shards within that partition (replication group) are not sufficient. This prevent writes from taking place on the wrong side of a network partition. Possible values are: one, quorum, and all.

**Sample request**

Following is a sample request that can be handled by the routeDocument operation.

```json
{
    "apiUrl": "http://localhost:9200",
    "pretty": "true",
    "human": "true",
    "filterPath": "_index,_type",
    "flatSettings": "true",
    "callback": "true",
    "case": "camelCase",
    "indexName": "twitter",
    "type": "tweet",
    "user": "kimchy",
    "postDate": "2009-11-15T14:12:12",
    "message": "trying out Elasticsearch",
    "routingName": "kimchy",
    "timeStamp": "2009-11-15T14:12:12",
    "timeToLive": "86400000",
    "timeout": "5m",
    "refresh": "true",
    "consistency": "quorum"
}
```

**Sample response**

Given below is a sample response for the routeDocument operation.

```json
{
   "_index":"1537433624954index_name",
   "_type":"tweet",
   "_id":"AWX2L25sKTdgaaIiclE4",
   "_version":1,
   "result":"created",
   "_shards":{
      "total":2,
      "successful":1,
      "failed":0
   },
   "created":true
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html#index-routing

#### Setting the child document index

The indexChildDocument operation sets the child document index.

**indexChildDocument**
```xml
<elasticsearch.indexChildDocument>
    <parentId>{$ctx:parentId}</parentId>
    <childId>{$ctx:childId}</childId>
    <indexName>{$ctx:indexName}</indexName>
    <tagValue>{$ctx:tagValue}</tagValue>
    <type>{$ctx:type}</type>
    <parentType>{$ctx:parentType}</parentType>
</elasticsearch.indexChildDocument>
```

**Properties**
* parentId: The ID of the parent document inside which the child document is tagged.
* childId: The ID of the child document to be indexed.
* indexName: The name of the index where the document is indexed.
* tagValue: Value of the tag to be added to the child document.
* type: The type of the child document.
* parentType: The type of the parent document.

**Sample request**

Following is a sample request that can be handled by the indexChildDocument operation.

```json
{
   "apiUrl": "http://172.22.217.169:9200/",
   "pretty":"true",
   "format":"yaml",
   "human":"true",
   "filterPath":"_index,_type",
   "flatSettings":"true",
   "case":"camelCase",
   "parentId": "2",
   "childId": "3",
   "indexName": "final",
   "tagValue": "something",
   "type": "child2",
   "parentType":"perent"   
}
```

**Sample response**

Given below is a sample response for the indexChildDocument operation.

```json
{
   "_index":"1537433624954index_man",
   "_type":"child90",
   "_id":"3",
   "_version":1,
   "result":"created",
   "_shards":{
      "total":2,
      "successful":1,
      "failed":0
   },
   "created":true
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-index_.html#parent-children

#### Retrieving a typed JSON document

The getDocument operation retrieves a typed JSON document from the index based on a specified ID.

**createAutomaticIndex**
```xml
<elasticsearch.getDocument>
    <indexName>{$ctx:indexName}</indexName>
    <type>{$ctx:type}</type>
    <id>{$ctx:id}</id>
    <source>{$ctx:source}</source>
    <sourceExclude>{$ctx:sourceExclude}</sourceExclude>
    <sourceInclude>{$ctx:sourceInclude}</sourceInclude>
    <fields>{$ctx:fields}</fields>
    <routing>{$ctx:routing}</routing>
    <preference>{$ctx:preference}</preference>
    <refresh>{$ctx:refresh}</refresh>
    <realtime>{$ctx:realtime}</realtime>
    <version>{$ctx:version}</version>
</elasticsearch.getDocument>
```

**Properties**
* indexName: The name of the index.
* type: The type of the document.
* id: The identifier of the document to be retrieved.
* source: The complete source.   <br/>
  **Note:**
  The complete source is returned by default unless you specify a set of fields via the fields property, or if the source property is disabled.
* sourceExclude: A comma separated list of fields or wildcard expressions to omit from the complete source.
* sourceInclude: A comma separated list of fields or wildcard expressions that needs to be included from the complete source.
* fields: The stored fields that should be returned.
* routing: The routing value of the document.
* preference: Specify the shard on which the operation should be performed.
* refresh: Set this to true if you want to refresh the relevant shard before the operation, so that the document is searchable. 
* realtime: Set this to true if you want changes of the operation to be reflected in real time. 
* version: Specify the version of the document you need to retrieve.


**Sample request**

Following is a sample request that can be handled by the getDocument operation.

```json
{
    "apiUrl": "http://localhost:9200",
    "indexName": "twitter",
    "type": "tweet",
    "id": "1",
    "source": "*.id,retweeted",
    "sourceExclude": "entities",
    "sourceInclude": "*.id",
    "fields": "title,content",
    "routing": "kimchy",
    "preference": "_primary",
    "refresh": "true",
    "realtime": "false",
    "version": "1",
    "pretty": "true",
    "human": "true",
    "filterPath": "_index,_type",
    "flatSettings": "true",
    "callback": "true",
    "case": "camelCase",
    "format":"yaml"
}
```

**Sample response**

Given below is a sample response for the getDocument operation.

```json
{
   "_index":"1537433624954index_name",
   "_type":"tweet",
   "_id":"documentIdOptional_1537433624954",
   "_version":23,
   "found":true,
   "_source":{
      "post_date":"2015-11-15T14:12:12",
      "user":"Kimchya",
      "message":"trying out Elasticsearch"
   }
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-get.html#docs-get

#### Deleting a typed JSON document 

The deleteDocument operation deletes a typed JSON document from an index based on a specified ID.

**createAutomaticId**
```xml
<elasticsearch.deleteDocument>
    <indexName>{$ctx:indexName}</indexName>
    <type>{$ctx:type}</type>
    <id>{$ctx:id}</id>
    <version>{$ctx:version}</version>
    <routing>{$ctx:routing}</routing>
    <refresh>{$ctx:refresh}</refresh>
    <parentId>{$ctx:parentId}</parentId>
    <consistency>{$ctx:consistency}</consistency>
    <timeout>{$ctx:timeout}</timeout>
</elasticsearch.deleteDocument>
```

**Properties**
* indexName: The name of the index.
* type: The type of the document.
* id: The identifier of the document to be deleted.
* routing: The routing value of the document.
* version: Specify the version of the document you need to delete.
* refresh: Set this to true if you want to refresh the relevant shard before the getDocument operation, so that the document is searchable. 
* parentId: The ID of the parent document inside which the child document is tagged.
* consistency: An appropriate value to prevent the operation from starting when the number of active shards within that partition (replication group) are not sufficient. This prevent writes from taking place on the wrong side of a network partition. Possible values are: one, quorum, and all.
* timeout: A timeout value for the operation. This is the duration that the index operation waits for the primary shard to become available, at a time that the primary shard assigned to perform the index operation is not available.

**Sample request**

Following is a sample request that can be handled by the deleteDocument operation.

```json
{
    "apiUrl": "http://localhost:9200",
    "indexName": "twitter",
    "type": "tweet",
    "id": "1",
    "version": "1",
    "routing": "kimchy",
    "refresh": "true",
    "parentId": "1111",
    "consistency": "quorum",
    "timeout": "5m",
    "pretty": "true",
    "human": "true",
    "filterPath": "_index,_type",
    "flatSettings": "true",
    "callback": "true",
    "case": "camelCase"
}
```

**Sample response**

Given below is a sample response for the deleteDocument operation.

```json
{
   "found":true,
   "_index":"1537433624954index_name",
   "_type":"tweet",
   "_id":"AWX2L2uNKTdgaaIiclE2",
   "_version":3,
   "result":"deleted",
   "_shards":{
      "total":2,
      "successful":1,
      "failed":0
   }
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-delete.html#docs-delete

#### Retrieving multiple documents 

The listDocuments operation retrieves multiple documents based on an index, type and ID.

**listDocuments**
```xml
<elasticsearch.listDocuments>
    <indexName>{$ctx:indexName}</indexName>
    <routing>{$ctx:routing}</routing>
    <refresh>{$ctx:refresh}</refresh>
    <realtime>{$ctx:realtime}</realtime>
    <fields>{$ctx:fields}</fields>
    <source>{$ctx:source}</source>
    <docs>{$ctx:docs}</docs>
    <ids>{$ctx:ids}</ids>
    <type>{$ctx:type}</type>
    <sourceExclude>{$ctx:sourceExclude}</sourceExclude>
    <sourceInclude>{$ctx:sourceInclude}</sourceInclude>
</elasticsearch.listDocuments>
```
**Properties**
* indexName: The name of the index where the document should be indexed.
* routing: The routing value of the document.
* refresh: Set this to true if you want to refresh the relevant shard before the getDocument operation, so that the document is searchable. 
* realtime: This is set to true by default, and is not affected by the refresh rate of the index. If you do not want changes of the operation to be reflected in real time, set this to false. 
* fields: The stored fields that should be returned.
* source: The complete source.  <br/>
  **Note:**
  The complete source is returned by default unless you specify a set of fields via the fields property, or if the source property is disabled. 
* docs: The documents to be retrieved.
* ids: The list of document identifiers to be retrieved.
* type: Set this to all or leave it empty in order to fetch the first document that matches the id across all types.
* sourceExclude: A comma separated list of fields or wildcard expressions to omit from the complete source.
* sourceInclude: A comma separated list of fields or wildcard expressions that needs to be included from the complete source.

**Sample request**

Following is a sample request that can be handled by the listDocuments operation.
```json
{
    "apiUrl": "http://localhost:9200",
    "indexName": "twitter",
    "type":"tweet",
    "fields": "post_date",
    "pretty": "true",
    "human": "true",
    "filterPath": "_index,_type",
    "flatSettings": "true",
    "callback": "true",
    "case": "camelCase",
    "format": "yaml",
    "docs": [
        {
            "_index" : "twitter",
            "_type": "tweet",
            "_id": "1",
            "_source" : "false"
             
        },
        {
            "_type": "tweet",
            "_id": "2",
            "fields": ["user","post_date"]
        },
        {
            "_type": "tweet",
            "_id": "3",
            "_source" : ["user", "post_date"]
        }
    ],
    "ids": ["1","2","3"]
}
```

**Sample response**

Given below is a sample response for the listDocuments operation.

```json
{
   "docs":[
      {
         "_index":"1537433624954index_name",
         "_type":"tweet",
         "_id":"documentIdMandatory_1537433624954",
         "_version":1,
         "found":true,
         "_source":{

         }
      },
      {
         "_index":"1537433624954index_name",
         "_type":"tweet",
         "_id":"documentIdOptional_1537433624954",
         "_version":23,
         "found":true,
         "_source":{
            "post_date":"2015-11-15T14:12:12",
            "user":"Kimchya",
            "message":"trying out Elasticsearch"
         }
      }
   ]
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-multi-get.html#docs-multi-get

#### Updating a document 

The updateDocument operation updates a document based on a script that is provided.

**updateDocument**
```xml
<elasticsearch.updateDocument>
    <indexName>{$ctx:indexName}</indexName>
    <type>{$ctx:type}</type>
    <id>{$ctx:id}</id>
    <script>{$ctx:script}</script>
    <params>{$ctx:params}</params>
    <timeout>{$ctx:timeout}</timeout>
    <consistency>{$ctx:consistency}</consistency>
    <parentId>{$ctx:parentId}</parentId>
    <routing>{$ctx:routing}</routing>
    <refresh>{$ctx:refresh}</refresh>
    <fields>{$ctx:fields}</fields>
    <version>{$ctx:version}</version>
    <versionType>{$ctx:versionType}</versionType>
    <retryOnConflict>{$ctx:retryOnConflict}</retryOnConflict>
    <detectNoop>{$ctx:detectNoop}</detectNoop>
    <upsert>{$ctx:upsert}</upsert>
    <scriptedUpsert>{$ctx:scriptedUpsert}</scriptedUpsert>
    <docAsUpsert>{$ctx:docAsUpsert}</docAsUpsert>
    <doc>{$ctx:doc}</doc>
</elasticsearch.updateDocument>
```

**Properties**
* indexName: The name of the index.
* type: The type of the document.
* id: The identifier of the document to be updated.
* script: The script used to update the document.
* params: The list of parameters to update the document.
* timeout: A timeout value for the operation. This is the duration that the index operation waits for the primary shard to become available, at a time that the primary shard assigned to perform the index operation is not available.
* consistency: An appropriate value to prevent the operation from starting when the number of active shards within that partition (replication group) are not sufficient. This prevent writes from taking place on the wrong side of a network partition. Possible values are: one, quorum, and all.
* parentId: The ID of the parent document inside which the child document is tagged.
* routing: The routing value of the document.
* refresh: Set this to true if you want to refresh the relevant shard before the getDocument operation, so that the document is searchable. 
* fields: The stored fields that should be returned.
* version: Specify the version of the document you need to update.
* versionType: The specific version type of the document. Possible values are: internal, external and external_gte.
* retryOnConflict: Specify the number of times to retry before throwing an exception when there is a conflict in updating the document. For example, it is possible that another process might have already updated the same document in between the get and indexing phases of the update. Specifying a value for retryOnConflict can control how many times to retry the update before throwing an exception.
* detectNoop: Set this to true if you want updates that do not change anything to be detected and converted to a noop.
* upsert: If the document does not already exist, the content of upsert is inserted as a new document. If the document does exist, then the script is executed instead.
* scriptedUpsert: Set this to true if you want the update script to run regardless of whether the document exists or not.
* docAsUpsert: Set this to true to use the content of the doc as the upsert value instead of sending a partial doc together with an upsert doc.
* doc: The document to update with.


**Sample request**

Following is a sample request that can be handled by the updateDocument operation.

```json
{
    "apiUrl": "http://localhost:9200",
    "pretty": "true",
    "human": "true",
    "filterPath": "_index,_type",
    "flatSettings": "true",
    "callback": "true",
    "case": "camelCase",
    "indexName": "twitter",
    "type": "tweet",
    "id": "1",
    "script": "ctx._source.counter += count",
    "params": {"count": 4},
    "timeout": "5m",
    "consistency": "quorum",
    "parentId": "1111",
    "routing": "kimchy",
    "refresh": "true",
    "fields": "title,content",
    "version": "2",
    "versionType": "external",
    "retryOnConflict": "true",
    "detectNoop": "true",
    "upsert": {"counter": 1},
    "scriptedUpsert": "true",
    "docAsUpsert": "true",
    "doc": {"name": "new_name"}
}
```

**Sample response**

Given below is a sample response for the updateDocument operation.

```json
{
   "_index":"1537433624954index_name",
   "_type":"tweet",
   "_id":"AWX2L2yRKTdgaaIiclE3",
   "_version":2,
   "result":"updated",
   "forced_refresh":true,
   "_shards":{
      "total":2,
      "successful":1,
      "failed":0
   }
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-update.html#docs-update

#### Performing multiple index/delete operations

The performBulkOperations operation performs multiple index/delete operations in a single API call.

**performBulkOperations**
```xml
<elasticsearch.performBulkOperations>
</elasticsearch.performBulkOperations>
```

**Sample request**

Following is a sample request that can be handled by the performBulkOperations operation.
```
http://localhost:8280/services/elasticsearch_performBulkOperations?apiUrl=http://172.22.217.169:9200&pretty=true&case=camelCase
```

**Sample response**

Given below is a sample response for the performBulkOperations operation.

```json
{
   "took":12,
   "errors":false,
   "items":[
      {
         "index":{
            "_index":"1537433624954index_name",
            "_type":"tweet",
            "_id":"AWX2L2qzKTdgaaIiclE1",
            "_version":1,
            "result":"created",
            "_shards":{
               "total":2,
               "successful":1,
               "failed":0
            },
            "created":true,
            "status":201
         }
      },
      {
         "delete":{
            "found":false,
            "_index":"1537433624954index_name",
            "_type":"tweet",
            "_id":"invalid",
            "_version":1,
            "result":"not_found",
            "_shards":{
               "total":2,
               "successful":1,
               "failed":0
            },
            "status":404
         }
      }
   ]
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/docs-bulk.html#docs-bulk

### Sample configuration

Following example illustrates how to connect to Elasticsearch with the init operation and createDocumentWithIndex operation.

1. Create a sample proxy as below :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="createDocumentWithIndex" startOnLoad="true" statistics="disable" trace="disable" transports="https,http" xmlns="http://ws.apache.org/ns/synapse">
   <target>
      <inSequence onError="faultHandlerSeq">
         <property name="apiUrl" expression="json-eval($.apiUrl)"/>
         <property name="pretty" expression="json-eval($.pretty)"/>
         <property name="format" expression="json-eval($.format)"/>
         <property name="human" expression="json-eval($.human)"/>
         <property name="filterPath" expression="json-eval($.filterPath)"/>
         <property name="flatSettings" expression="json-eval($.flatSettings)"/>
         <property name="callback" expression="json-eval($.callback)"/>
         <property name="case" expression="json-eval($.case)"/>
         <property name="indexName" expression="json-eval($.indexName)"/>
         <property name="type" expression="json-eval($.type)"/>
         <property name="id" expression="json-eval($.id)"/>
         <property name="user" expression="json-eval($.user)"/>
         <property name="postDate" expression="json-eval($.postDate)"/>
         <property name="message" expression="json-eval($.message)"/>
         <property name="version" expression="json-eval($.version)"/>
         <property name="versionType" expression="json-eval($.versionType)"/>
         <property name="operationType" expression="json-eval($.operationType)"/>
         <property name="timeStamp" expression="json-eval($.timeStamp)"/>
         <property name="timeToLive" expression="json-eval($.timeToLive)"/>
         <property name="timeout" expression="json-eval($.timeout)"/>
         <property name="refresh" expression="json-eval($.refresh)"/>
         <property name="consistency" expression="json-eval($.consistency)"/>
         <elasticsearch.init>
            <apiUrl>{$ctx:apiUrl}</apiUrl>
            <pretty>{$ctx:pretty}</pretty>
            <format>{$ctx:format}</format>
            <human>{$ctx:human}</human>
            <filterPath>{$ctx:filterPath}</filterPath>
            <flatSettings>{$ctx:flatSettings}</flatSettings>
            <callback>{$ctx:callback}</callback>
            <case>{$ctx:case}</case>
         </elasticsearch.init>
         <elasticsearch.createDocumentWithIndex>
            <indexName>{$ctx:indexName}</indexName>
            <type>{$ctx:type}</type>
            <id>{$ctx:id}</id>
            <user>{$ctx:user}</user>
            <postDate>{$ctx:postDate}</postDate>
            <message>{$ctx:message}</message>
            <version>{$ctx:version}</version>
            <versionType>{$ctx:versionType}</versionType>
            <operationType>{$ctx:operationType}</operationType>
            <timeStamp>{$ctx:timeStamp}</timeStamp>
            <timeToLive>{$ctx:timeToLive}</timeToLive>
            <timeout>{$ctx:timeout}</timeout>
            <refresh>{$ctx:refresh}</refresh>
            <consistency>{$ctx:consistency}</consistency>
         </elasticsearch.createDocumentWithIndex>
         <respond/>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </target>
   <description/>
</proxy>
```
2. Create an json file named createDocumentWithIndex.json and copy the configurations given below to it:

```json
{
    "apiUrl": "http://localhost:9200",
    "pretty": "true",
    "format": "yaml",
    "human": "true",
    "filterPath": "_index,_type",
    "flatSettings": "true",
    "callback": "true",
    "case": "camelCase",
    "indexName": "twitter",
    "type": "tweet",
    "id": "402",
    "user": "kimchy",
    "postDate": "2009-11-15T14:12:12",
    "message": "trying out Elasticsearch",
    "version": "2",
    "versionType": "external",
    "operationType": "create",
    "timeStamp": "2009-11-15T14:12:12",
    "timeToLive": "86400000",
    "timeout": "5m",
    "refresh": "true",
    "consistency": "quorum"
}
```

3. Replace the credentials with your values.

4. Execute the following curl command:

```bash
curl http://localhost:8280/services/createDocumentWithIndex -H "Content-Type: application/json" -d @createDocumentWithIndex.json
```
5. Elasticsearch returns an json response similar to the one shown below:
 
```json
{
   "_index":"1537433624954index_name",
   "_type":"tweet",
   "_id":"documentIdOptional_1537433624954",
   "_version":23,
   "result":"created",
   "_shards":{
      "total":2,
      "successful":1,
      "failed":0
   },
   "created":true
}
```