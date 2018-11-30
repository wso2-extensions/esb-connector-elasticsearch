# Configuring Elasticsearch Operations

[[Prerequisites]](#Prerequisites) [[Initializing the Connector]](#initializing-the-connector)

## Prerequisites

To use the Elasticsearch connector, add the <elasticsearch.init> element in your configuration before carrying out any other Elasticsearch operations. 

For more information on how to setup an Elasticsearch instance, see https://www.elastic.co/guide/en/elasticsearch/reference/current/setup.html.

## Initializing the Connector

Specify the init method as follows:

**init**
```xml
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
```
**Properties** 
* apiUrl:  The base end point URL of Elasticsearch API.
* pretty:  The boolean flag to indicate whether the JSON response is to be returned as pretty formatted.
* format:  Indicates whether the results to be returned in more readable YAML format.
* human:  The boolean flag to indicate whether the response is to be returned in human readable format.
* filterPath:  The comma separated list of filters expressed with the dot notation.
* flatSettings:  The boolean flag to indicate whether the response is to be returned in the flat format.
* callback:  The boolean flag to indicate whether the response is to be returned as JSONP result.
* case:  When set to camelCase, all field names in the result will be returned in camel casing, otherwise, underscore casing will be used.

Now that you have connected to Elasticsearch, use the information in the following topics to perform various operations with the connector:

[Working with Documents in Elasticsearch](documents.md)

[Working with Search in Elasticsearch](search.md)
