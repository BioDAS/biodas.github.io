---
title: DAS/1
permalink: wiki/DAS/1/
layout: wiki
tags:
 - Documentation
---

<big>**The DAS Protocol**</big>

This page describes

About the DAS Protocol
----------------------

The current DAS specification, [DAS
1.53](http://www.biodas.org/documents/spec.html), was written by Lincoln
Stein, Sean Eddy, and Robin Dowell and is the basis for a large number
of software implementations. Around 650 public DAS data sources are
currently running worldwide, with many more private data sources known
to be in use. A number of websites and software applications function as
DAS clients.

The specification is being actively supported, and continues to be
extended in order to cater for the needs of its existing users and
expand its applicability to additional arenas. For example, though
originally focussed on genomic annotation, extensions have enabled DAS
to be used to distribute alignment, structural and molecular interaction
data. These extensions are listed in the unofficial [1.53E (extended)
specification](http://www.dasregistry.org/spec_1.53E.jsp).

<b>Note:</b> This protocol should not be confused with that of
[DAS/2](/wiki/DAS/2 "wikilink"), an entirely separate project.

The [DAS Overview](/wiki/DAS/1/Overview "wikilink") provides a glossary and
list of concepts. The [DAS Plans](/wiki/DAS_Plans "wikilink") page describes
some details of possible future improvements to DAS.

Architecture
------------

The Distributed Annotation System comprises three types of component:

### Data source

A data source provides programmatic access to data over a network,
typically the internet. Each <i>data source</i> contains a set of data
from one provider, for example Pfam domains.

### Registry

The [ DAS Registry](/wiki/DasRegistry "wikilink") lists and describes public
<i>data sources</i> and the types of data that may be communicated using
DAS. It is accessible programatically.

### Server

The term <i>DAS server</i> is often used interchangeably with <i>DAS
source</i>. However a server is technically a piece of software used to
host one or more <i>data sources</i>, and like the <i>registry</i> can
provide details of these <i>data sources</i>.

### Client

A client consumes and integrates the data contained within one or more
<i>data sources</i>. It may also communicate with a <i>server</i> or
<i>registry</i> to obtain information about available data sources.

How to set up a DAS server
--------------------------

### Implementation

Theoretically it is quite easy to implement a DAS server (once you know
how). However, there are also some well established multi-purpose server
implementations designed to be as flexible and easy to set up as
possible. Many distributions contain ready made data-adaptors (e.g. for
GFF files). For custom data, simple plugins can be written to quickly
provide your data via DAS:

-   [Proserver](http://www.sanger.ac.uk/proserver/) is a well
    established Perl server, supports all DAS extensions and is the most
    heavily used.
-   [LDAS](http://biodas.org/servers/LDAS.html) is an older Perl server
    which is easy to set up but lacks support for the latest
    DAS features.
-   [Dazzle](http://www.biojava.org/wiki/Dazzle) is a well established
    Java server tied to the BioJava framework and support
    most extensions.
-   [MyDas](http://code.google.com/p/mydas/) is a newer, more
    streamlined Java server but does not yet support all extensions.

### Validation

Servers should endeavour to conform strictly to the DAS data formats as
this maximises compatibility across the network and minimises the
maintenance required as a result of evolving client software. RelaxNG
documents are now available from the [DasRegistry DAS
Registry](/wiki/DasRegistry_DAS_Registry "wikilink") to help validate the XML
produced by your servers. These are used by the registry to help servers
conform to the DAS specification before being registered.

### Publishing and Discovery of DAS sources

Publishing your source in the DAS Registry allows you to advertise its
availability, and is described in the [DAS
Registry](/wiki/DasRegistry "wikilink") documentation. Registered sources are
also easier for users to visualise in clients that are capable of
interacting with the DAS Registry, such as
[Ensembl](http://www.ensembl.org/), [Dasty](http://www.ebi.ac.uk) and
[SPICE](http://www.efamily.org.uk/software/dasclients/spice/).

Training in DAS
---------------

DAS Tutorials, talks and hackathons take place once a year at the Genome
Campus UK almost every year in the [DAS
Workshop](http://www.sanger.ac.uk/Software/analysis/das/DASWorkshopHistory.shtml).
The next workshop is likely to be in March 2010. Events, courses and
workshops involving DAS are often listed on the [Current
events](/wiki/Current_events "wikilink") page.
