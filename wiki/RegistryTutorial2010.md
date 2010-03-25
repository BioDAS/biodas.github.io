---
title: RegistryTutorial2010
permalink: wiki/RegistryTutorial2010/
layout: wiki
---

Tutorial: An Introduction to the DAS Registry
=============================================

Introduction
------------

The next section describes how you can manually find DAS services.

Finding DAS sources – the DAS Registry Service at the Sanger Institute
----------------------------------------------------------------------

<center>
**<http://www.dasregistry.org>**

</center>
*“The purpose of the DAS registration service is to keep track which DAS
services are around, which DAS commands they are understanding and about
the coordinate systems of their data.”*

The DAS Registration Server allows DAS clients to discover DAS services.
At the same time it provides an elegant browsable web interface that
allows you to search for DAS sources, test their status, examine their
reliability and learn more about what they offer. At the time of
writing, a total of 729 servers from 44 institutions in 44 different
countries are included in the registry.

The following activities will familiarize you with the content of the
registry, both the 'human' interface and also the web-service interface
that can be queried by DAS clients (and possibly your client code in the
exercises this afternoon).

First of all, a few exercises to familiarize you with the DAS Registry
web site:

> ***Navigate to [The DAS Registration
> Server](http://www.dasregistry.org)***

> ***Follow the "list" ... "list sources" menu item at the top of this
> page. On the resulting page, find all DAS annotation servers that use
> UniProt as their 'authority'.***

> ***Select any one of these services and follow the information link
> ('i' in a blue circle) to find out details of this server including
> how reliable it has been recently.***

> ***For the same DAS service, use the DAS Registration Server to test
> its current capabilities to find out if it is supporting all of the
> functionality that it claims to at the present time. (Start with the
> green tick).***

The DAS Registration Server allows DAS sources to be organised into
projects, which can also be used at the client level to request
annotation from logically associated DAS sources, such as DAS sources
that are associated with the BioSapiens Project.

> ***To view all the projects currently described in the DAS
> Registration Server, take a look at:
> <http://www.dasregistry.org/listProjects.jsp>***

Of course different DAS data sources serve sequences and features based
upon numerous different accession and coordinate systems. The DAS
Registration Server provides functionality to keep track of this and to
ensure a common nomenclature.

> ***Look at the response to the DAS Registry coordsys command:
> <http://www.dasregistry.org/coordsys/CS_DS6>. On this page you are
> presented with a description of a specific accession system, namely
> the UniProt coordinate system in this case. You will also find a link
> that allows you to view a list of all the DAS servers that implement
> this coordinate / accession system.***

The next few exercises will allow you to examine some of the *web
service* capabilities of the DAS Registration Server.

Note that the DAS Registration Service provides both a simple DAS style
web service allowing you build a simple URL request and receive an XML
response, as well as a SOAP (Simple Object Access Protocol) web service.
Here we will focus on the DAS style web service.

For the DAS style web service functionality provided by the DAS
Registration Service, XSLT style sheets are used to transform the XML
response into an attractive web page for viewing in an internet browser.
For all of the following activities, please look at both the view in the
web browser as well as the source of the page, which will reveal the XML
response that can be parsed in suitable client code.

> ***Look at the response to the DAS Registry sources command:
> <http://www.dasregistry.org/das1/sources>. As indicated above, the
> view you are seeing in the browser is a transformation of the XML
> response into an attractive HTML view. Right click on the browser
> window and click on 'View Source'. (The exact details of this depend
> upon the internet browser you are using).***

You should see that the XML response provides a `<SOURCES>` XML file,
containing any number of `<SOURCE>` elements. Each `<SOURCE>` element
describes a single data source, providing parsable information
including:

-   The source URI as defined in the registry, of the form DS\_NNN;
-   The title and a description of the data source;
-   Details of the datasource maintainer;
-   The coordinate system used by the data source (note that this is
    defined in terms of DAS Registry registered coordinate systems, e.g.
    "UniProt" has been assigned the URI CS\_DS6);
-   The capabilities (implemented commands) of the data source;
-   A range of additional properties for the service.

> ***You can also perform the sources query for a single datasource:
> <http://www.dasregistry.org/das1/sources/DS_409>***

For details of web services and WSDL:

> ***Finally, you may like to take a look at the [DAS Registry Server
> Help with Scripting
> Page](http://www.dasregistry.org/help_scripting.jsp) that provides
> more information about how to use the available web services in your
> own scripts.***

<small>Philip Jones, EMBL-EBI, Hinxton, UK. February 2008</small>