---
title: DAS1.6
permalink: wiki/DAS1.6/
layout: wiki
---

**Note: this is NOT the official version of the [DAS
specification](http://biodas.org/documents/spec.html).**

Distributed Annotation System (DAS) 1.6
=======================================

This is a working document and a proposal for a reworked DAS
specification which hopes to:

-   clarify the existing DAS spec
-   better reflect how DAS is being used in the community today
-   better reflect the use of DAS beyond the genome-centric arena
    (protein sequences and structures)
-   ratify commands from the [1.53E
    spec](http://www.dasregistry.org/spec_1.53E.jsp)
-   introduce some ideas inspired by DAS/2
-   formally introduce the DAS registry
-   migrate from the use of DTDs for describing the XML formats to
    RelaxNG

Note: the DAS specification is a technical document but with some effort
should hopefully be readable and understandable by people without a deep
understanding of broader technical issues and other system
architectures. That is, it makes only basic assumptions.

The specification has been written as a number of drafts:

-   [Draft 1](http://www.ebi.ac.uk/~aj/1.6_draft1/documents/spec.html)
-   [Draft 2](http://www.ebi.ac.uk/~aj/1.6_draft2/documents/spec.html)
-   [Draft 3](http://www.ebi.ac.uk/~aj/1.6_draft3/documents/spec.html)
-   [Draft 5](http://www.ebi.ac.uk/~aj/1.6_draft5/documents/spec.html)

Last modified: 29th Jan 2010

(DAS) 1.6 Development and Tutorials
===================================

Dazzle Example adaptor for 1.6 spec source compliant with the registry
----------------------------------------------------------------------

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

The new Dazzle implements the single Data Source sources.xml request:
blah/das/myDataSourceName. The current implementation relies on on a
valid sources.xml document being available and then parses the
appropriate section from this document in response to a request.
