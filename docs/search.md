# Working with Search in Elasticsearch

[[Overview]](#overview)  [[Operation details]](#operation-details)  [[Sample configuration]](#sample-configuration)

### Overview 

The following operations allow you to work with search. Click an operation name to see details on how to use it.
For a sample proxy service that illustrates how to work with searches, see [Sample configuration](#sample-configuration).

| Operation        | Description |
| ------------- |-------------|
| [searchDocumentsByIndex](#searching-all-documents-across-all-types-within-a-specified-index)    | Performs a search on all documents across all types within a specified index. |
| [searchDocumentByTypes](#searching-all-documents-within-a-specified-index-based-on-specified-types)      | Performs a search on all documents documents within a specified index based on specified types.  |
| [searchByQuery](#searching-across-all-indices-and-all-types)    | Performs a search across all indices and all types. |
| [multiSearch](#executing-multiple-search-requests)    | Executes multiple search requests within the same API.    |

### Operation details

This section provides further details on the operations related to search.

#### Searching all documents across all types within a specified index

The searchDocumentsByIndex operation searches all documents across all types within a specified index.

**searchDocumentsByIndex**
```xml
<elasticsearch.searchDocumentsByIndex>
    <indexName>{$ctx:indexName}</indexName>
    <query>{$ctx:query}</query>
    <explain>{$ctx:explain}</explain>
    <fields>{$ctx:fields}</fields>
    <sort>{$ctx:sort}</sort>
    <timeout>{$ctx:timeout}</timeout>
    <terminateAfter>{$ctx:terminateAfter}</terminateAfter>
    <from>{$ctx:from}</from>
    <size>{$ctx:size}</size>
    <searchType>{$ctx:searchType}</searchType>
    <source>{$ctx:source}</source>
    <scriptFields>{$ctx:scriptFields}</scriptFields>
    <fieldDataFields>{$ctx:fieldDataFields}</fieldDataFields>
    <highlight>{$ctx:highlight}</highlight>
    <indicesBoost>{$ctx:indicesBoost}</indicesBoost>
    <minScore>{$ctx:minScore}</minScore>
    <trackScores>{$ctx:trackScores}</trackScores>
    <version>{$ctx:version}</version>
    <scroll>{$ctx:scroll}</scroll>
    <queryCache>{$ctx:queryCache}</queryCache>
    <partialFields>{$ctx:partialFields}</partialFields>
    <postFilter>{$ctx:postFilter}</postFilter>
    <aggregation>{$ctx:aggregation}</aggregation>
    <rescore>{$ctx:rescore}</rescore>
    <preference>{$ctx:preference}</preference>
    <innerHits>{$ctx:innerHits}</innerHits>
    <ignoreUnavailable>{$ctx:ignoreUnavailable}</ignoreUnavailable>
    <allowNoIndices>{$ctx:allowNoIndices}</allowNoIndices>
    <expandWildcards>{$ctx:expandWildcards}</expandWildcards>
</elasticsearch.searchDocumentsByIndex>
```

**Properties**
* indexName: The name of the index.
* query : The query string used to perform the search.
* explain: Set this to true if you want detailed information about score computation returned as part of a hit.
* fields: A comma separated list of fields to return for each hit.
* sort: The type of sorting to perform. Can either be in the form of fieldName, or fieldName:asc/fieldName:desc. The fieldName can either be an actual field within the document, or the special score name to indicate sorting based on scores. There can be several sort parameters (order is important). 
* timeout: An operation timeout value for the search. This causes the search to execute within the specified time and returns the hits accumulated within that period of time. 
* terminateAfter: The maximum number of documents to collect for each shard, which upon reaching causes the query execution to terminate early. If set, the response will have a boolean field terminated_early to indicate that the query execution has terminated early. 
* from: The starting offset. Default value is 0.
* size: The number of hits to return. Default value is 10.
* searchType: The search operation type. Possible values are: dfs_query_then_fetch, dfs_query_and_fetch, query_then_fetch, query_and_fetch, count, scan. The default value is: query_then_fetch. For more information on the different types of search that can be performed, see [Search Type](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-search-type.html).
* source: Set this to true if you want the source fields returned in the response.
* scriptFields: Provide a script that can work on fields that are not stored, and return custom values for each hit based on the script evaluation. 
* fieldDataFields: A comma separated list of fields to return as the field data representation of a field for each hit.
* highlight: Specify the fields that should be highlighted in each search hit.
* indicesBoost: Allows to configure different boost levels per index when searching across more than one index. This is useful when hits coming from a particular index have a higher priority over hits coming from another index (social graph where each user has an index).
* minScore: Specify a value so that documents with a score less than the specified value are excluded from the search.
* trackScores: Set this to true if you want to calculate and return scores even if they are not used for sorting.
* version: Set this to true if you want the document version returned for each hit.
* scroll: Specify how long a consistent view of the index should be maintained for scrolled search. The duration should be specified in time units. For information on supported time units, see [Time units](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#time-units).
* queryCache: Set this to true if you want to enable caching of search results for requests.
* partialFields: Specify wildcards based on which parts of the original document you want returned for each hit.
* postFilter: If you need to differentially filter search hits, specify the search values here. Values other than what is specified here will be removed from the search hits.
* aggregation: Specify the document fields from which the values should be extracted for aggregation.
* rescore: A second query to be executed on the results returned by the original query.
* preference: Specify the shard on which the search operation should be performed.
* innerHits: Specify a definition that returns additional nested hits that caused a search hit to satisfy a different scope, for each search hit in the search response.
* ignoreUnavailable: Set this to true if you want to ignore any index that is unavailable.
* allowNoIndices: Set this to true if you want to ignore an index when a wildcard index expression results in no concrete index.
* expandWildcards: Set this to true if you want to expand wildcard expression to concrete indices.

**Sample request**

Following is a sample request that can be handled by the searchDocumentsByIndex operation.

```json
{
  "apiUrl": "http://172.22.217.169:9200",
  "indexName": "twitter",
  "query": {
    "term": {
      "tag": "something"
    }
  },
  "explain": "true",
  "fields": [
    "message"
  ],
  "sort": [
    {
      "_type": "asc"
    }
  ],
  "timeout": "12",
  "terminateAfter": "12",
  "from": "12",
  "size": "1",
  "searchType": "count",
  "source": "false",
  "scriptFields": {
    "my_field": {
      "script": "1 + my_var",
      "params": {
        "my_var": 2
      }
    }
  },
  "fieldDataFields": [
    "user"
  ],
  "highlight": {
    "fields": {
      "content": {}
    }
  },
  "indicesBoost": {
    "index1": "1.4",
    "index2": "1.3"
  },
  "minScore": "1",
  "trackScores": "true",
  "version": "true",
  "scroll": "1m",
  "queryCache": "true",
  "partialFields": {
    "partial1": {
      "include": "obj1"
    }
  },
  "postFilter": {
    "term": {
      "tag": "something"
    }
  },
  "aggregation": {
    "tag": {
      "terms": {
        "field": "tag"
      }
    }
  },
  "rescore": {
    "window_size": "10",
    "query": {
      "rescore_query": {
        "match": {
          "tag": {
            "query": "something"
          }
        }
      },
      "query_weight": 0.7,
      "rescore_query_weight": 1.2
    }
  },
  "preference": "_shards:2",
  "innerHits": {
    "comment": {
      "path": {
        "comments": {
          "query": {
            "match": {
              "comments.message": "[different query]"
            }
          }
        }
      }
    }
  }, 
  "pretty":"true",
  "format":"yaml",
  "human":"true",
  "filterPath":"_index,_type",
  "flatSettings":"true",
  "callback":"true",
  "case":"camelCase",
  "ignoreUnavailable":"true",
  "allowNoIndices":"true",
  "expandWildcards":"true"
}
```

**Sample response**

Given below is a sample response for the searchDocumentsByIndex operation.

```json
{
    "timed_out": false,
    "took": 62,
    "_shards":{
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
    },
    "hits":{
        "total" : 1,
        "max_score": 1.3862944,
        "hits" : [
            {
                "_index" : "twitter",
                "_type" : "_doc",
                "_id" : "0",
                "_score": 1.3862944,
                "_source" : {
                    "user" : "kimchy",
                    "date" : "2009-11-15T14:12:12",
                    "message" : "trying out Elasticsearch",
                    "likes": 0
                }
            }
        ]
    }
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html

#### Searching all documents within a specified index based on specified types

The searchDocumentByTypes operation performs a search on all documents within a specified index based on specified types.

**searchDocumentByTypes**
```xml
<elasticsearch.searchDocumentByTypes>
    <indexName>{$ctx:indexName}</indexName>
    <query>{$ctx:query}</query>
    <explain>{$ctx:explain}</explain>
    <fields>{$ctx:fields}</fields>
    <sort>{$ctx:sort}</sort>
    <timeout>{$ctx:timeout}</timeout>
    <terminateAfter>{$ctx:terminateAfter}</terminateAfter>
    <from>{$ctx:from}</from>
    <size>{$ctx:size}</size>
    <searchType>{$ctx:searchType}</searchType>
    <source>{$ctx:source}</source>
    <scriptFields>{$ctx:scriptFields}</scriptFields>
    <fieldDataFields>{$ctx:fieldDataFields}</fieldDataFields>
    <highlight>{$ctx:highlight}</highlight>
    <indicesBoost>{$ctx:indicesBoost}</indicesBoost>
    <minScore>{$ctx:minScore}</minScore>
    <trackScores>{$ctx:trackScores}</trackScores>
    <version>{$ctx:version}</version>
    <scroll>{$ctx:scroll}</scroll>
    <type>{$ctx:type}</type>
    <queryCache>{$ctx:queryCache}</queryCache>
    <partialFields>{$ctx:partialFields}</partialFields>
    <postFilter>{$ctx:postFilter}</postFilter>
    <aggregation>{$ctx:aggregation}</aggregation>
    <rescore>{$ctx:rescore}</rescore>
    <preference>{$ctx:preference}</preference>
    <innerHits>{$ctx:innerHits}</innerHits>
    <ignoreUnavailable>{$ctx:ignoreUnavailable}</ignoreUnavailable>
    <allowNoIndices>{$ctx:allowNoIndices}</allowNoIndices>
    <expandWildcards>{$ctx:expandWildcards}</expandWildcards>
</elasticsearch.searchDocumentByTypes>
```

**Properties**
* indexName: The name of the index.
* query : The query string used to perform the search.
* explain: Set this to true if you want detailed information about score computation returned as part of a hit.
* fields: A comma separated list of fields to return for each hit.
* sort: The type of sorting to perform. Can either be in the form of fieldName, or fieldName:asc/fieldName:desc. The fieldName can either be an actual field within the document, or the special score name to indicate sorting based on scores. There can be several sort parameters (order is important). 
* timeout: An operation timeout value for the search. This causes the search to execute within the specified time and returns the hits accumulated within that period of time. 
* terminateAfter: The maximum number of documents to collect for each shard, which upon reaching causes the query execution to terminate early. If set, the response will have a boolean field terminated_early to indicate that the query execution has terminated early. 
* from: The starting offset. Default value is 0.
* size: The number of hits to return. Default value is 10.
* searchType: The search operation type. Possible values are: dfs_query_then_fetch, dfs_query_and_fetch, query_then_fetch, query_and_fetch, count, scan. The default value is: query_then_fetch. For more information on the different types of search that can be performed, see [Search Type](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-search-type.html).
* source: Set this to true if you want the source fields returned in the response.
* scriptFields: Provide a script that can work on fields that are not stored, and return custom values for each hit based on the script evaluation. 
* fieldDataFields: A comma separated list of fields to return as the field data representation of a field for each hit.
* highlight: Specify the fields that should be highlighted in each search hit.
* indicesBoost: Allows to configure different boost levels per index when searching across more than one index. This is useful when hits coming from a particular index have a higher priority over hits coming from another index (social graph where each user has an index).
* minScore: Specify a value so that documents with a score less than the specified value are excluded from the search.
* trackScores: Set this to true if you want to calculate and return scores even if they are not used for sorting.
* version: Set this to true if you want the document version returned for each hit.
* scroll: Specify how long a consistent view of the index should be maintained for scrolled search. The duration should be specified in time units. For information on supported time units, see [Time units](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#time-units).
* queryCache: Set this to true if you want to enable caching of search results for requests.
* partialFields: Specify wildcards based on which parts of the original document you want returned for each hit.
* postFilter: If you need to differentially filter search hits, specify the search values here. Values other than what is specified here will be removed from the search hits.
* aggregation: Specify the document fields from which the values should be extracted for aggregation.
* rescore: A second query to be executed on the results returned by the original query.
* preference: Specify the shard on which the search operation should be performed.
* innerHits: Specify a definition that returns additional nested hits that caused a search hit to satisfy a different scope, for each search hit in the search response.
* ignoreUnavailable: Set this to true if you want to ignore any index that is unavailable.
* allowNoIndices: Set this to true if you want to ignore an index when a wildcard index expression results in no concrete index.
* expandWildcards: Set this to true if you want to expand wildcard expression to concrete indices.

**Sample request**

Following is a sample request that can be handled by the searchDocumentByTypes operation.

```json
{
  "apiUrl": "http://localhost:9200",
  "indexName": "twitter",
  "type": "tweet",
  "query": {
    "term": {
      "tag": "something"
    }
  },
  "explain": "true",
  "fields": [
    "message"
  ],
  "sort": [
    {
      "_type": "asc"
    }
  ],
  "timeout": "12",
  "terminateAfter": "1",
  "from": "2",
  "size": "1",
  "searchType": "count",
  "source": "false",
  "scriptFields": {
    "my_field": {
      "script": "1 + my_var",
      "params": {
        "my_var": 2
      }
    }
  },
  "fielddataFields": [
    "user"
  ],
  "highlight": {
    "fields": {
      "content": {}
    }
  },
  "indicesBoost": {
    "index1": "1.4",
    "index2": "1.3"
  },
  "trackScore": "true",
  "minScore": "6",
  "version": "false",
  "scroll": "1m",
  "type": "tweet",
  "queryCache": "true",
  "partialFields": {
    "partial1": {
      "include": "obj1"
    }
  },
  "postFilter": {
    "term": {
      "tag": "something"
    }
  },
  "aggregation": {
    "tag": {
      "terms": {
        "field": "tag"
      }
    }
  },
  "rescore": {
    "window_size": "10",
    "query": {
      "rescore_query": {
        "match": {
          "tag": {
            "query": "something"
          }
        }
      },
      "query_weight": 0.7,
      "rescore_query_weight": 1.2
    }
  },
  "preference": "_shards:2",
  "innerHits": {
    "comment": {
      "path": {
        "comments": {
          "query": {
            "match": {
              "comments.message": "[different query]"
            }
          }
        }
      }
    }
  },
  "pretty":"true",
  "format":"yaml",
  "human":"true",
  "filterPath":"_index,_type",
  "flatSettings":"true",
  "callback":"true",
  "case":"camelCase",
  "ignoreUnavailable":"true",
  "allowNoIndices":"true",
  "expandWildcards":"true"
}

```

**Sample response**

Given below is a sample response for the searchDocumentByTypes operation.

```json
{
   "took":3,
   "timed_out":false,
   "_shards":{
      "total":5,
      "successful":5,
      "skipped":0,
      "failed":0
   },
   "hits":{
      "total":5,
      "max_score":1.0,
      "hits":[
         {
            "_index":"1537433624954index_name",
            "_type":"tweet",
            "_id":"AWX2L2qzKTdgaaIiclE1",
            "_score":1.0,
            "_source":{
               "post_date":"2015-11-15T14:12:12"
            }
         },
         .
         .
      ]
   }
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html

#### Searching across all indices and all types

The searchByQuery operation performs a search across all indices and all types.

**searchByQuery**
```xml
<elasticsearch.searchByQuery>
    <query>{$ctx:query}</query>
    <explain>{$ctx:explain}</explain>
    <fields>{$ctx:fields}</fields>
    <sort>{$ctx:sort}</sort>
    <timeout>{$ctx:timeout}</timeout>
    <terminateAfter>{$ctx:terminateAfter}</terminateAfter>
    <from>{$ctx:from}</from>
    <size>{$ctx:size}</size>
    <searchType>{$ctx:searchType}</searchType>
    <source>{$ctx:source}</source>
    <scriptFields>{$ctx:scriptFields}</scriptFields>
    <fieldDataFields>{$ctx:fieldDataFields}</fieldDataFields>
    <highlight>{$ctx:highlight}</highlight>
    <indicesBoost>{$ctx:indicesBoost}</indicesBoost>
    <minScore>{$ctx:minScore}</minScore>
    <trackScores>{$ctx:trackScores}</trackScores>
    <version>{$ctx:version}</version>
    <scroll>{$ctx:scroll}</scroll>
    <queryCache>{$ctx:queryCache}</queryCache>
    <partialFields>{$ctx:partialFields}</partialFields>
    <postFilter>{$ctx:postFilter}</postFilter>
    <aggregation>{$ctx:aggregation}</aggregation>
    <rescore>{$ctx:rescore}</rescore>
    <preference>{$ctx:preference}</preference>
    <innerHits>{$ctx:innerHits}</innerHits>
</elasticsearch.searchByQuery>
```

**Properties**
* indexName: The name of the index.
* query : The query string used to perform the search.
* explain: Set this to true if you want detailed information about score computation returned as part of a hit.
* fields: A comma separated list of fields to return for each hit.
* sort: The type of sorting to perform. Can either be in the form of fieldName, or fieldName:asc/fieldName:desc. The fieldName can either be an actual field within the document, or the special score name to indicate sorting based on scores. There can be several sort parameters (order is important). 
* timeout: An operation timeout value for the search. This causes the search to execute within the specified time and returns the hits accumulated within that period of time. 
* terminateAfter: The maximum number of documents to collect for each shard, which upon reaching causes the query execution to terminate early. If set, the response will have a boolean field terminated_early to indicate that the query execution has terminated early. 
* from: The starting offset. Default value is 0.
* size: The number of hits to return. Default value is 10.
* searchType: The search operation type. Possible values are: dfs_query_then_fetch, dfs_query_and_fetch, query_then_fetch, query_and_fetch, count, scan. The default value is: query_then_fetch. For more information on the different types of search that can be performed, see [Search Type](https://www.elastic.co/guide/en/elasticsearch/reference/current/search-request-search-type.html).
* source: Set this to true if you want the source fields returned in the response.
* scriptFields: Provide a script that can work on fields that are not stored, and return custom values for each hit based on the script evaluation. 
* fieldDataFields: A comma separated list of fields to return as the field data representation of a field for each hit.
* highlight: Specify the fields that should be highlighted in each search hit.
* indicesBoost: Allows to configure different boost levels per index when searching across more than one index. This is useful when hits coming from a particular index have a higher priority over hits coming from another index (social graph where each user has an index).
* minScore: Specify a value so that documents with a score less than the specified value are excluded from the search.
* trackScores: Set this to true if you want to calculate and return scores even if they are not used for sorting.
* version: Set this to true if you want the document version returned for each hit.
* scroll: Specify how long a consistent view of the index should be maintained for scrolled search. The duration should be specified in time units. For information on supported time units, see [Time units](https://www.elastic.co/guide/en/elasticsearch/reference/current/common-options.html#time-units).
* queryCache: Set this to true if you want to enable caching of search results for requests.
* partialFields: Specify wildcards based on which parts of the original document you want returned for each hit.
* postFilter: If you need to differentially filter search hits, specify the search values here. Values other than what is specified here will be removed from the search hits.
* aggregation: Specify the document fields from which the values should be extracted for aggregation.
* rescore: A second query to be executed on the results returned by the original query.
* preference: Specify the shard on which the search operation should be performed.
* innerHits: Specify a definition that returns additional nested hits that caused a search hit to satisfy a different scope, for each search hit in the search response.
* ignoreUnavailable: Set this to true if you want to ignore any index that is unavailable.
* allowNoIndices: Set this to true if you want to ignore an index when a wildcard index expression results in no concrete index.
* expandWildcards: Set this to true if you want to expand wildcard expression to concrete indices.

**Sample request**

Following is a sample request that can be handled by the searchByQuery operation.

```json
{
  "apiUrl": "http://172.22.217.169:9200",
  "query": {
    "term": {
      "tag": "something"
    }
  },
  "explain": "true",
  "fields": [
    "name"
  ],
  "sort": [
    {
      "_type": "asc"
    }
  ],
  "timeout": "12",
  "terminateAfter": "12",
  "from": "12",
  "size": "1",
  "searchType": "count",
  "source": "false",
  "scriptFields": {
    "my_field": {
      "script": "1 + my_var",
      "params": {
        "my_var": 2
      }
    }
  },
  "fieldDataFields": [
    "user"
  ],
  "highlight": {
    "fields": {
      "content": {}
    }
  },
  "indicesBoost": {
    "index1": "1.4",
    "index2": "1.3"
  },
  "minScore": "1",
  "trackScores": "true",
  "version": "true",
  "scroll": "1m",
  "queryCache": "true",
  "partialFields": {
    "partial1": {
      "include": "obj1"
    }
  },
  "postFilter": {
    "term": {
      "tag": "something"
    }
  },
  "aggregation": {
    "tag": {
      "terms": {
        "field": "tag"
      }
    }
  },
  "rescore": {
    "window_size": "10",
    "query": {
      "rescore_query": {
        "match": {
          "tag": {
            "query": "something"
          }
        }
      },
      "query_weight": 0.7,
      "rescore_query_weight": 1.2
    }
  },
  "preference": "_shards:2",
  "innerHits": {
    "comment": {
      "path": {
        "comments": {
          "query": {
            "match": {
              "comments.message": "[different query]"
            }
          }
        }
      }
    }
  },
  "pretty":"true",
  "format":"yaml",
  "human":"true",
  "filterPath":"_index,_type",
  "flatSettings":"true",
  "callback":"true",
  "case":"camelCase"
}
```

**Sample response**

Given below is a sample response for the searchByQuery operation.

```json
{
   "took":34,
   "timed_out":false,
   "_shards":{
      "total":28,
      "successful":28,
      "skipped":0,
      "failed":0
   },
   "hits":{
      "total":12,
      "max_score":1.0,
      "hits":[
         {
            "_index":"1537433624954index_name",
            "_type":"tweet",
            "_id":"AWX2L2qzKTdgaaIiclE1",
            "_score":1.0,
            "_source":{
               "post_date":"2015-11-15T14:12:12"
            }
         },
         .
         .
      ]
   }
}
```
**Related Elasticsearch documentation**
https://www.elastic.co/guide/en/elasticsearch/reference/current/search-search.html

#### Executing multiple search requests 

The multiSearch operation executes multiple search requests within the same API.

**multiSearch**
```xml
<elasticsearch.multiSearch>
</elasticsearch.multiSearch>
```

**Sample request**

Following is a sample request that can be handled by the multiSearch operation.

```
http://localhost:8280/services/elasticsearch_multiSearch?apiUrl=http://172.22.217.169:9200&pretty=true&case=camelCase
```

**Sample response**

Given below is a sample response for the multiSearch operation.

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
https://www.elastic.co/guide/en/elasticsearch/reference/current/search-multi-search.html 

### Sample configuration

Following example illustrates how to connect to Elasticsearch with the init operation and searchDocumentsByIndex operation.

1. Create a sample proxy as below :

```xml
<?xml version="1.0" encoding="UTF-8"?>
<proxy name="searchDocumentsByIndex" startOnLoad="true" statistics="disable" trace="disable" transports="https,http" xmlns="http://ws.apache.org/ns/synapse">
   <target>
      <inSequence onError="faultHandlerSeq">
         <property name="apiUrl" expression="json-eval($.apiUrl)"/>
         <property name="indexName" expression="json-eval($.indexName)"/>
         <property name="query" expression="json-eval($.query)"/>
         <property name="explain" expression="json-eval($.explain)"/>
         <property name="fields" expression="json-eval($.fields)"/>
         <property name="sort" expression="json-eval($.sort)"/>
         <property name="timeout" expression="json-eval($.timeout)"/>
         <property name="terminateAfter" expression="json-eval($.terminateAfter)"/>
         <property name="from" expression="json-eval($.from)"/>
         <property name="size" expression="json-eval($.size)"/>
         <property name="searchType" expression="json-eval($.searchType)"/>
         <property name="source" expression="json-eval($.source)"/>
         <property name="scriptFields" expression="json-eval($.scriptFields)"/>
         <property name="fieldDataFields" expression="json-eval($.fieldDataFields)"/>
         <property name="highlight" expression="json-eval($.highlight)"/>
         <property name="indicesBoost" expression="json-eval($.indicesBoost)"/>
         <property name="minScore" expression="json-eval($.minScore)"/>
         <property name="trackScores" expression="json-eval($.trackScores)"/>
         <property name="version" expression="json-eval($.version)"/>
         <property name="scroll" expression="json-eval($.scroll)"/>
         <property name="queryCache" expression="json-eval($.queryCache)"/>
         <property name="partialFields" expression="json-eval($.partialFields)"/>
         <property name="postFilter" expression="json-eval($.postFilter)"/>
         <property name="aggregation" expression="json-eval($.aggregation)"/>
         <property name="rescore" expression="json-eval($.rescore)"/>
         <property name="preference" expression="json-eval($.preference)"/>
         <property name="innerHits" expression="json-eval($.innerHits)"/>
         <property name="pretty" expression="json-eval($.pretty)"/>
         <property name="format" expression="json-eval($.format)"/>
         <property name="human" expression="json-eval($.human)"/>
         <property name="filterPath" expression="json-eval($.filterPath)"/>
         <property name="flatSettings" expression="json-eval($.flatSettings)"/>
         <property name="callback" expression="json-eval($.callback)"/>
         <property name="case" expression="json-eval($.case)"/>
         <property name="ignoreUnavailable" expression="json-eval($.ignoreUnavailable)"/>
         <property name="allowNoIndices" expression="json-eval($.allowNoIndices)"/>
         <property name="expandWildcards" expression="json-eval($.expandWildcards)"/>
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
         <elasticsearch.searchDocumentsByIndex>
            <indexName>{$ctx:indexName}</indexName>
            <query>{$ctx:query}</query>
            <explain>{$ctx:explain}</explain>
            <fields>{$ctx:fields}</fields>
            <sort>{$ctx:sort}</sort>
            <timeout>{$ctx:timeout}</timeout>
            <terminateAfter>{$ctx:terminateAfter}</terminateAfter>
            <from>{$ctx:from}</from>
            <size>{$ctx:size}</size>
            <searchType>{$ctx:searchType}</searchType>
            <source>{$ctx:source}</source>
            <scriptFields>{$ctx:scriptFields}</scriptFields>
            <fieldDataFields>{$ctx:fieldDataFields}</fieldDataFields>
            <highlight>{$ctx:highlight}</highlight>
            <indicesBoost>{$ctx:indicesBoost}</indicesBoost>
            <minScore>{$ctx:minScore}</minScore>
            <trackScores>{$ctx:trackScores}</trackScores>
            <version>{$ctx:version}</version>
            <scroll>{$ctx:scroll}</scroll>
            <queryCache>{$ctx:queryCache}</queryCache>
            <partialFields>{$ctx:partialFields}</partialFields>
            <postFilter>{$ctx:postFilter}</postFilter>
            <aggregation>{$ctx:aggregation}</aggregation>
            <rescore>{$ctx:rescore}</rescore>
            <preference>{$ctx:preference}</preference>
            <innerHits>{$ctx:innerHits}</innerHits>
            <ignoreUnavailable>{$ctx:ignoreUnavailable}</ignoreUnavailable>
            <allowNoIndices>{$ctx:allowNoIndices}</allowNoIndices>
            <expandWildcards>{$ctx:expandWildcards}</expandWildcards>
         </elasticsearch.searchDocumentsByIndex>
         <respond/>
      </inSequence>
      <outSequence>
         <send/>
      </outSequence>
   </target>
   <description/>
</proxy>

```
2. Create an json file named searchDocumentsByIndex.json and copy the configurations given below to it:

```json
{ 
   "providerUrl":"ldap://localhost:10389/",
   "securityPrincipal":"cn=admin,dc=wso2,dc=com",
   "securityCredentials":"comadmin",
   "secureConnection":"false",
   "disableSSLCertificateChecking":"false",
   "application":"ldap",
   "operation":"createEntity",
   "content":{ 
      "objectClass":"inetOrgPerson",
      "dn":"uid=testDim20,ou=staff,dc=wso2,dc=com",
      "attributes":{ 
         "mail":"testDim1s22c@wso2.com",
         "userPassword":"12345",
         "sn":"dim",
         "cn":"dim",
         "manager":"cn=dimuthuu,ou=Groups,dc=example,dc=com"
      }
   }
}
```

3. Replace the credentials with your values.

4. Execute the following curl command:

```bash
curl http://localhost:8280/services/searchDocumentsByIndex -H "Content-Type: application/json" -d @searchDocumentsByIndex.json
```
5. Elasticsearch returns an json response similar to the one shown below:
 
```json
{
    "timed_out": false,
    "took": 62,
    "_shards":{
        "total" : 1,
        "successful" : 1,
        "skipped" : 0,
        "failed" : 0
    },
    "hits":{
        "total" : 1,
        "max_score": 1.3862944,
        "hits" : [
            {
                "_index" : "twitter",
                "_type" : "_doc",
                "_id" : "0",
                "_score": 1.3862944,
                "_source" : {
                    "user" : "kimchy",
                    "date" : "2009-11-15T14:12:12",
                    "message" : "trying out Elasticsearch",
                    "likes": 0
                }
            }
        ]
    }
}
```