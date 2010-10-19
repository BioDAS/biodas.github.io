---
title: DAS 1.6 in Dazzle
permalink: wiki/DAS_1.6_in_Dazzle/
layout: wiki
---

DAS 1.6 Development and Tutorials
=================================

Dazzle Example Adaptor
----------------------

Below is an example adaptor plugin for the Dazzle DAS server. It follows
the 1.6 spec and is compliant with the [DAS
Registry](/wiki/DasRegistry "wikilink").

Subversion develpment repository
<http://www.derkholm.net/svn/repos/dazzle/branches/16Dazzle> See class
org.biojava.servlets.dazzle.datasource.SimpleFile16ExampleSource for a
datasource that conforms to the new 1.6 Spec.

    <datasource id="test16genes" jclass="org.biojava.servlets.dazzle.datasource">
         <string name="name" value="16 File Test" />
         <string name="description" value="Data source for testing 1.6 heirachy structure" />
         <string name="version" value="1.0" /> 
         
         <string name="fileName" value="test16genes.txt" />

         <string name="stylesheet" value="test16.style" />
       </datasource>

Note: Older style adapters/datasources can be used in the new 16Dazzle
and will be converted to the new 1.6 specification (you may need to add
a allTypes method). However you will miss out on some of the advantages
of the new spec if you update your source by this method rather than
extending one of the new 16 classes.

The new Dazzle implements the single Data Source sources.xml request:
blah/das/myDataSourceName. The current implementation relies on on a
valid sources.xml document being available and then parses the
appropriate section from this document in response to a request.
