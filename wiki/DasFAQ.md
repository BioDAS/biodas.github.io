---
title: DasFAQ
permalink: wiki/DasFAQ/
layout: wiki
tags:
 - Documentation
---

<big>**DAS Frequently Asked Questions**</big>

Which version of DAS should I be using?
---------------------------------------

In short, "DAS" usually refers to the original version of the protocol.
It continues to evolve and be supported, and is the basis of
implementations for the vast majority of clients and servers. "DAS/2" is
a separate protocol that is inspired by many of the same principles as
DAS, but is not backwards compatible. Though the specification is now
"frozen" and therefore considered stable, there exist few solid
implementations. However, many of of the technical improvements in DAS/2
are now beginning to be adopted in DAS.

What applications are the actual "DAS clients"?
-----------------------------------------------

DAS applications generally take the form of graphical genome browsers,
protein feature/structure browsers, and even gene information browsers.

For an example of how Ensembl acts as a DAS client for gene information
browsing, see the "Gene DAS Report" section of this page:
<http://www.ensembl.org/Homo_sapiens/geneview?gene=ENSG00000165029>

Here's an interesting review of EBI DAS clients:
<http://www.ebi.ac.uk/~rafael/das/index.php?display=clients>

See also: [\#What DAS client should I
use?](#What_DAS_client_should_I_use? "wikilink")

What DAS client should I use?
-----------------------------

Examples of DAS applications are listed on both the [DAS
Clients](/wiki/DAS/1#Clients "wikilink") and
[DAS/2\#Clients](/wiki/DAS/2#Clients "wikilink") pages.

Are DAS and DAS/2 designed to interoperate? For example, will I be able to use a DAS/2 client and a DAS server?
---------------------------------------------------------------------------------------------------------------

DAS/2 is a complete redesign of the original DAS specification, so
direct interoperation is not possible. However, the [DAS/2
spec](http://biodas.org/documents/das2/das2_protocol.html) has all of
the capabilities of the [DAS
spec](http://www.biodas.org/documents/spec.html) (and more!).

A proxy adapter is being developed that will provide a DAS/2 interface
around an existing DAS server, allowing DAS/2 clients to interact with
existing DAS servers:
<http://lists.open-bio.org/pipermail/das2/2006-October/000867.html>

To fully realize 1 &lt;-&gt; 2 interoperation, there would need to be a
DAS proxy adapter for DAS/2 servers, to permit DAS clients to interact
with DAS/2 servers. Whether such a utility will be needed or developed
remains to be seen.

I am totally confused, what should I do?
----------------------------------------

Unfortunately due to the wide application and abstract nature of DAS, it
can be difficult to find information at the right level for each
person's individual perspective. It is highly recommended to ask
questions on one of the mailing lists, see the [Community
Portal](/wiki/BioDAS:Community_Portal "wikilink") for details.

How does one retrieve genomic coordinates from a list of gene names or Entrez Gene IDs using DAS/2?
---------------------------------------------------------------------------------------------------

See this thread on the DAS/2 discussion list:
<http://lists.open-bio.org/pipermail/das2/2007-January/000967.html>
