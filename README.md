# Payload Component

This plugin for Solr will return the payloads for terms that matched in your query, and was developed to meet the needs of doing (Keyword in Context Highlighting)[https://en.wikipedia.org/wiki/Key_Word_in_Context] of OCR'ed content. You can see it in action in this demonstration project: https://github.com/o19s/pdf-discovery-demo/.

Example document:

```
{
  "id": "my sample doc",
  "payload_content": "Look|ignored at this|wow"
}
```

Querying for `payload_content:this` would generate a response like the following:

```
{
  "response":{
    "docs":[
      {
        "id":"my sample doc",
        "payload_content":"Look|ignored at this|wow",
      }
    ]
  },
  "payloads":{
    "my sample_doc":{
      "payload_content":{
        "this":[
          "wow"
        ]
      }
    }
  }
}     
```
Since `wow` was a payload of the `this` token, and `this` was in the query, `wow` comes back in the payloads response.

## Why?
This project was originally conceived as a solution for storing bounding boxes with terms for OCR highlighting.

See it in action at http://github.com/o19s/pdf-discovery-demo.

## Requirements
- Solr 8.7
- A field type that utilizes payloads

# Installing as a Solr Package
To install as a Solr package, first ensure Solr has been started with `-Denable.packages=true` then add the repo:

`bin/solr package add-repo osc https://raw.githubusercontent.com/o19s/payload-component/master/repo`

To install the package run:

`bin/solr package install solr-payloads:1.1.4`

## Payload Component Usage
- Add the payload component to the `last-components` section of your search handler
- Configure a field type that utilizes payloads
- Pass `pl=on` in your query params for queries in which you want to extract payload matches
- To learn how to configure you Solr review the test `schema.xml` and `solrconfig.xml` at https://github.com/o19s/payload-component/tree/master/src/test/resources/solr/collection1/conf

## Offset Highlighter Usage
Frequently we need to do highlighting in OCR offset from a specific location.  This is done in the demonstration project: https://github.com/o19s/pdf-discovery-demo/.

This plugin provides an `OffsetFormatter` which supports pre tags with `$score`, `$numTokens`, `$endOffset`, `$startOffset` placeholders.  If these placeholders are included in the tag they will be replaced with the tokengroup score and offset for the matching text.

Check out the test configuration files for an idea of how to get set up in an actual solr instance.


## Building
Building requires JDK 8 and Maven.  Once you're setup just run:

`mvn package` to generate the latest jar in the target folder.

-------------

__This component was first dreamed up at the [September 2019 Lucene Hackday](https://opensourceconnections.com/blog/2019/09/23/what-happens-at-a-lucene-solr-hackday/).__
