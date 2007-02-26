---
title: DasFAQ
permalink: wiki/DasFAQ/
layout: wiki
---

<big>**DAS FAQ**</big>

What applications are the actual "DAS clients"?
-----------------------------------------------

DAS applications generally take the form of graphical genome browsers,
protein feature/structure browsers, and even gene information browsers.

Examples of DAS applications are listed on both the
[DAS/1\#Clients](/wiki/DAS/1#Clients "wikilink") and
[DAS/2\#Clients](/wiki/DAS/2#Clients "wikilink") pages.

For an example of how Ensembl acts as a DAS client for gene information
browsing, see the "Gene DAS Report" section of this page:
<http://www.ensembl.org/Homo_sapiens/geneview?gene=ENSG00000165029>

Here's an interesting review of EBI DAS clients:
<http://www.ebi.ac.uk/~rafael/das/index.php?display=clients>

Are DAS/1 and DAS/2 designed to interoperate? For example, will I be able to use a DAS/2 client and a DAS/1 server?
-------------------------------------------------------------------------------------------------------------------

DAS/2 is a complete redesign of the original DAS/1 specification, so
direct interoperation is not possible. However, the [DAS/2
spec](http://biodas.org/documents/das2/das2_protocol.html) has all of
the capabilities of the [DAS/1
spec](http://www.biodas.org/documents/spec.html) (and more!).

As proof of this, a proxy adapter is being developed that will provide a
DAS/2 interface around an existing DAS/1 server, allowing DAS/2 clients
to interact with existing DAS/1 servers:
<http://lists.open-bio.org/pipermail/das2/2006-October/000867.html>

To fully realize 1 &lt;-&gt; 2 interoperation, there would need to be a
DAS/1 proxy adapter for DAS/2 servers, to permit DAS/1 clients to
interact with DAS/2 servers. Whether such a utility will be needed or
developed remains to be seen.

How does one retrieve genomic coordinates from a list of gene names or Entrez Gene IDs using DAS/2?
---------------------------------------------------------------------------------------------------

See this thread on the DAS/2 discussion list:
<http://lists.open-bio.org/pipermail/das2/2007-January/000967.html>
