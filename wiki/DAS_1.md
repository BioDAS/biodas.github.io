---
title: DAS/1
permalink: wiki/DAS/1/
layout: wiki
tags:
 - Documentation
---

<big>**The DAS/1 Protocol**</big>

About DAS/1
-----------

The original version 1 specification, written by Lincoln Stein, Sean
Eddy, and Robin Dowell, is the basis for a number of clients and
servers. Around 400 public DAS/1 servers are currently running worldwide
including [WormBase](http://www.wormbase.org/),
[FlyBase](http://www.flybase.org/), [Ensembl](http://www.ensembl.org/),
[TIGR](http://www.tigr.org/), [UCSC](http://genome.ucsc.edu/), and
[UniProt](http://www.ebi.ac.uk/das-srv/uniprot/das). Many more private
DAS sources are known to be in use. A number of websites and software
applications are based on DAS.

The official DAS/1 version 1.53 specification is available at
<http://www.biodas.org/documents/spec.html>

The specification is being actively supported, and continues to be
extended in order to cater for the needs of its existing users and
expand its applicability to additional arenas. For example, though
originally focussed on genomic annotation, extensions have enabled DAS
to be used to distribute alignment, structural and molecular interaction
data.

The unofficial DAS/1 version 1.53E (extended) specification is available
at <http://www.dasregistry.org/spec_1.53E.jsp>

[DAS/1/Overview](/wiki/DAS/1/Overview "wikilink") provides a glossary and list
of concepts. [DAS Plans](/wiki/DAS_Plans "wikilink") has some details of
possible future improvements to DAS.

DAS/1 Clients
-------------

-   [Ensembl](http://www.ensembl.org/info/using/external_data/das/index.html)
-   [Spice](http://www.efamily.org.uk/software/dasclients/spice/)
-   [Dasty](http://www.ebi.ac.uk/dasty/)
-   [Pfam](http://pfam.sanger.ac.uk/)

<!-- -->

-   Older (still maintained?):
    -   [Geodesic](http://biodas.org/geodesic/)
    -   [OmniDAS/OmniGene](http://sourceforge.net/project/showfiles.php?group_id=28453&release_id=60810)

DAS/1 Servers
-------------

A more exchaustive list of servers is available from the [DAS
Registry](/wiki/DasRegistry "wikilink").

-   [Affymetrix](http://netaffxdas.affymetrix.com/das/)
-   [BioSapiens
    servers](http://www.biosapiens.info/page.php?page=biosapiensdir)
-   [Ensembl
    server](http://www.ensembl.org/info/using/external_data/das/index.html)
-   [KEGG DAS](http://das.hgc.jp/)
-   [Sanger DAS server](http://das.sanger.ac.uk/das/dsn)
-   [EBI Genomic DAS
    server](http://www.ebi.ac.uk/das-srv/genomicdas/das/sources)
-   [EBI Protein DAS
    server](http://www.ebi.ac.uk/das-srv/proteindas/das/sources)
-   [Uniprot DAS server](http://www.ebi.ac.uk/das-srv/uniprot/das/dsn)
-   [TIGR's listing of
    servers](http://www.tigr.org/tdb/DAS/das_server_list.html)
-   [UCSC server](http://genome.ucsc.edu/FAQ/FAQdownloads#download23)

How to set up a DAS/1 server
----------------------------

In general it is quite easy to set up DAS server. All the server
implementations are easy to set up. Most server implementations allow
easy setup using ready provided data-adaptors (e.g. for GFF files). For
custom data simple plugins can be written to quickly provide your data
via DAS.

DAS server implementations are available in several programming
languages:

-   Perl

[`Proserver`](http://www.sanger.ac.uk/proserver/)  
[`LDAS`](http://biodas.org/servers/LDAS.html)

-   Java

[`Dazzle`](http://www.biojava.org/wiki/Dazzle)  
[`MyDas`](http://code.google.com/p/mydas/)

-   Validation

Validation RelaxNG documents are now available to help validate the xml
produced by your servers. It is intended that these will be used by the
registry to help servers conform to the DAS1 specification before being
registered.

Publishing and Discovery of DAS/1 sources
-----------------------------------------

See [DasRegistry](/wiki/DasRegistry "wikilink")
