---
title: Handling XML files
date: 2017-04-05 16:00:00 Z
---

# XML data format
XML (eXtensible Markup Language) is a syntax for storing and transferring data. It is a common data format for APIs. XML data is stored in a systematic hierarchy of tags. While JSON format has being increasingly preferred over XML format, XML is still used widely and provides some advantages over the former.

XML data is mostly hidden from regular Workato users because we expose data as input fields and output datapills in the recipe. However, you may occassionally require raw XML data for your use case. Workato provides a few options for handling such instances.

```xml
<request>
  <control>
    <senderid>SENDER_ID</senderid>
    <password>SENDER_PASSWORD</password>
    <controlid>testControlId</controlid>
    <uniqueid>false</uniqueid>
    <dtdversion>3.0</dtdversion>
  </control>
  <operation>
    <authentication>
      <login>
        <userid>USERNAME</userid>
        <companyid>COMPANY_ID</companyid>
        <password>USER_PASSWORD</password>
      </login>
    </authentication>
    <content>
      <function controlid="testControlId">
        <get_list object="glaccount" maxitems="1"></get_list>
      </function>
    </content>
  </operation>
</request>
```

This is a sample request body to Intacct Web services. The XML data is stored in a top level tag `<request>`, which has nested tags `<control>` and `<operation>`, which in turn, has respective nested tags. Because of this feature, XML data, when coupled with an adequate query language, is capable of storing and transporting deeply nested data.

On top of that, the ability to define **named-tags** as well as attributes within tags allows XML to create and work with very complex data structure, even creating new data types.

## XML Parser

The easiest way to convert an XML document into a format that can be used in Workato recipes is to parse it using the XML Parser by Workato. Refer to the [XML Parser documentation]() for more details.

## HTTP connector

The HTTP connector is a convenient way to handle raw data from APIs. Refer to our [HTTP connector course](http://resources.workato.com/http-connector/#/?_k=1szm77) for a detailed guide and examples on building HTTP triggers and actions to handle XML data.

## Workato SDK

Workato SDK provides developers with fewer constraints when building connectors to APIs. It is an extension of the Workato framework. It supports a variety of authentication procedures and allows developers to easily build, maintain and distribute connectors to integration-seekers. The Workato SDK is not just a framework to serve external developers. Many of the existing Application connectors on the Workato platform are built on top of this framework. Workato aims to leverage this robust and powerful framework constantly and improve it's capabilities.

Please refer to the main [Workato SDK documentation](/developing-connectors/sdk.md) for more details.

## Which should I use?

Refer to our documentation on the [HTTP vs SDK](/developing-connectors/http-vs-sdk.md) for a detailed comparison of the two options.
