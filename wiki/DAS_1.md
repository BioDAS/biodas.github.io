---
title: DAS/1
permalink: wiki/DAS/1/
layout: wiki
tags:
 - Documentation
---

About the DAS Protocol
----------------------

The DAS protocol was written by Lincoln Stein, Sean Eddy, and Robin
Dowell in 2000, and is the basis for a large number of software
implementations. Around 650 public DAS data sources are currently
running worldwide, with many more private data sources known to be in
use. A number of websites and software applications function as DAS
clients.

As of 19th October 2010, the latest version of the DAS protocol is [DAS
1.6](/wiki/DAS1.6 "wikilink").

The specification is being actively supported, and continues to be
extended in order to cater for the needs of its existing users and
expand its applicability to additional arenas. For example, though
originally focussed on genomic annotation, the latest version includes
support for protein sequences, structures and annotations. In addition,
extensions have enabled DAS to be used to distribute alignment and
molecular interaction data. These and other extensions are listed in the
unofficial [DAS 1.6E](/wiki/DAS1.6E "wikilink") specification.

<b>Note:</b> This protocol should not be confused with that of
[DAS/2](/wiki/DAS/2 "wikilink"), an entirely separate project.

The [DAS Overview](/wiki/DAS/1/Overview "wikilink") provides a glossary and
list of concepts. The [DAS Plans](/wiki/DAS_Plans "wikilink") page describes
some details of possible future improvements to DAS. See the [DAS
specification](/wiki/DAS_specification "wikilink") page for more information
on the different versions of DAS.

Architecture
------------

The Distributed Annotation System comprises three types of component:

### Data source

A data source provides programmatic access to data over a network,
typically the internet. Each <i>data source</i> contains a set of data
from one provider, for example Pfam domains. One or more <i>data
sources</i> may be hosted by a single <i>DAS server</i>.

### Registry

The [ DAS Registry](/wiki/DasRegistry "wikilink") lists and describes public
<i>data sources</i> and the types of data that may be communicated using
DAS. It is accessible programatically.

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
possible. It is strongly suggested to use one of these existing
packages. Many distributions contain ready made data-adaptors (e.g. for
GFF files). For custom data, simple plugins can be written to quickly
provide your data via DAS:

-   [ProServer](http://www.sanger.ac.uk/proserver/) is a well
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
-   [EasyDAS](http://www.ebi.ac.uk/panda-srv/easydas/) is a wrapper
    around ProServer facilitating access to the EBI's compute power for
    those that don't have access to a server of their own.

### Validation

Servers should endeavour to conform strictly to the DAS data formats as
this maximises compatibility across the network and minimises the
maintenance required as a result of evolving client software. RelaxNG
documents are now available from the [DAS
Registry](/wiki/DasRegistry "wikilink") to help validate the XML produced by
your servers. These are used by the registry to help servers conform to
the DAS specification before being registered.

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
Workshop](/wiki/DASWorkshop2011 "wikilink"). The next workshop will be in
March 2011. Events, courses and workshops involving DAS are often listed
on the [Current events](/wiki/Current_events "wikilink") page.
