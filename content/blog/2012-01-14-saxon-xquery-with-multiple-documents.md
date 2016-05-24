---
title: Saxon XQuery With Multiple Documents
author: Mashooq Badar
date: 2012-01-14T20:53:44Z

image:
  src: http://mashb.files.wordpress.com/2012/01/xquery.jpg
tags:
  - xquery
  - java
  - saxon

type: blog
layout: post
slug: saxon-xquery-with-multiple-documents
aliases: 
  - /2012/01/14/saxon-xquery-with-multiple-documents/
---

Saxon is a wonderful API for XML processing. It provides complete support for XPath, XQuery and XSLT. Although I'm always baffled with it's lack of adoption compared to Xalan and Xerces. Having said that the online documentation can definitely do with some improvement.  The following is a quick example of of how you may execute an XQuery that takes multiple XML documents as input.
~~~java
@Test
public void runXQueryWithMultipleInputDocuments() throws SaxonApiException {
    Processor processor = new Processor(false);

    DocumentBuilder documentBuilder = processor.newDocumentBuilder();
    XdmNode document = documentBuilder.build(
            new StreamSource(new StringReader("<my><document>content</document></my>")));
    XdmNode document2 = documentBuilder.build(
            new StreamSource(new StringReader("<my><document>content2</document></my>")));

    XQueryCompiler xQueryCompiler = processor.newXQueryCompiler();
    XQueryExecutable xQueryExecutable = xQueryCompiler.compile(
            "declare variable $mydoc external; " +
            "declare variable $mydoc2 external; " +
            "<newdoc><doc1>{$mydoc/my/document/text()}</doc1>" +
            "<doc2>{$mydoc2/my/document/text()}</doc2></newdoc>");

    XQueryEvaluator xQueryEvaluator = xQueryExecutable.load();

    QName name = new QName("mydoc");
    xQueryEvaluator.setExternalVariable(name, document);
    QName name2 = new QName("mydoc2");
    xQueryEvaluator.setExternalVariable(name2, document2);

    System.out.println(xQueryEvaluator.evaluate());
}
~~~

This result is an output of:
~~~xml
  <newdoc>
   <doc1>content</doc1>
   <doc2>content2</doc2>
</newdoc>
~~~
