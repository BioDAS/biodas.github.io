---
title: DAS/1
permalink: wiki/DAS/1/
layout: wiki
---

<big>**The DAS/1 Protocol**</big>

About DAS/1
-----------

The original version 1 specification, written by Lincoln Stein and Robin
Dowell, is the basis for a number of clients and servers. More than 200
DAS/1 servers are currently running worldwide including
[WormBase](http://www.wormbase.org/),
[FlyBase](http://www.flybase.org/), [Ensembl](http://www.ensembl.org/),
[TIGR](http://www.tigr.org/), [UCSC](http://genome.ucsc.edu/), and
[UniProt](http://www.ebi.ac.uk/das-srv/uniprot/das). A number of
websites and software applications are based on DAS.

The DAS/1 specification is currently available in version 1.53 from
<http://www.biodas.org/documents/spec.html>

Clients
-------

-   [Ensembl](http://www.ensembl.org/info/data/external_data/das/ensembl_das.html)
-   [Spice](http://www.efamily.org.uk/software/dasclients/spice/)
-   \[<http://www.ebi.ac.uk/das-srv/uniprot/dasty/content?ID>=:dis=BioSapiens
    Dasty\]
-   [Dasty2](http://www.ebi.ac.uk/~rafael/pre_dasty2/)

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

[`Proserver`](http://www.sanger.ac.uk/Software/analysis/proserver/)  
[`LDAS`](http://biodas.org/servers/LDAS.html)

-   Java

[`Dazzle`](http://www.derkholm.net/thomas/dazzle/)

Publishing and Discovery of DAS/1 sources
-----------------------------------------

See [DasRegistry](/wiki/DasRegistry "wikilink")
