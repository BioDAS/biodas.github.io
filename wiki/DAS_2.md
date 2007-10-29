---
title: DAS/2
permalink: wiki/DAS/2/
layout: wiki
tags:
 - Documentation
---

<big>**The DAS/2 Protocol**</big>

About DAS/2
-----------

DAS/2 is a more flexible and powerful version of the original
[DAS/1](/wiki/DAS/1 "wikilink"). The DAS/2 code base consists of:

1.  [The DAS/2
    specification](http://biodas.org/documents/das2/das2_protocol.html)
2.  [A publically accessible DAS/2 server
    implementation](http://das.biopackages.net/das/genome) (open source
    code available from the [GMOD project](http://www.gmod.org/))
3.  [An open-source DAS/2 client
    implementation](http://genoviz.sourceforge.net)
4.  [An open-source client library and server validation
    suite](http://sourceforge.net/projects/dasypus)

See [DAS/2\#CVS Access](/wiki/DAS/2#CVS_Access "wikilink") for details on
retrieving code related to these projects.

### DAS/2 History

DAS/2 evolved from [DAS/1](/wiki/DAS/1 "wikilink") through a community-driven
process, informed by a variety of [RFCs (Requests for
Comment)](http://biodas.org/RFCs/index.html) received from the community
of users and implementers of the spec. As of November 2006, the
[retrieval portion of the DAS/2
spec](http://biodas.org/documents/das2/das2_get.html) has stabilized.
Implementers and service providers can rely on this for their
DAS/2-based development efforts.

DAS/2 development officially commenced in May 2005 when funding for a
2-year
\[<http://crisp.cit.nih.gov/crisp/CRISP_LIB.getdoc?textkey=6712060&p_grant_num=1R01HG003040-01&p_query>=(DAS)&ticket=15416031&p\_audit\_session\_id=72191905&p\_audit\_score=100&p\_audit\_numfound=1&p\_keywords=DAS
NIH grant\] became available (the grant was actually approved in July
2004, but it took a while for NIH to decide the specific funding
amount). The grant officially expired at the end of October 2007.
Participating in the DAS/2 grant were Affymetrix, Cold Spring Harbor
Lab, the European Bioinformatics Institute/Sanger Center, and Dalke
Scientific.

Given that the DAS/2 specification is not backward compatible with the
DAS/1 version, existing DAS software will continue to support DAS/1. A
[DAS proxy server is in
development](http://lists.open-bio.org/pipermail/das2/2006-October/000867.html)
that will permit a DAS/1 server to be accessed by DAS/2 clients.

DAS/2 Clients
-------------

The following software packages operate as clients capable of
interacting with servers supporting the DAS/2 protocol:

-   [IGB - Integrated Genome Browser](http://genoviz.sourceforge.net) -
    a DAS/2 reference implementation
-   [GBrowse - Generic Genome Browser](http://www.gmod.org/GBrowse)

DAS/2 Servers
-------------

The following software packages operate as servers capable of providing
data in response to queries conforming with the DAS/2 protocol:

-   [Biopackages DAS/2 server](http://das.biopackages.net/das/genome) -
    a DAS/2 reference implementation
-   [Affymetrix Public DAS/2
    server](http://netaffxdas.affymetrix.com/das2)

DAS/2 Validation Suite
----------------------

See [Das2Validation](/wiki/Das2Validation "wikilink").

Publishing and Discovery of DAS/2 sources
-----------------------------------------

See [DasRegistry](/wiki/DasRegistry "wikilink")

Global Sequence Identifiers
---------------------------

See [GlobalSeqIDs](/wiki/GlobalSeqIDs "wikilink")

Feedback and Bug Reports
------------------------

A general forum for advice on DAS/2-related issues is the [discussion
list](http://biodas.org/mailman/listinfo/das2).

To submit or view bug reports in DAS/2-related software, use one of the
links below, depending on the affected component.

-   DAS/2 Specification:
    -   <http://bugzilla.open-bio.org/>
-   Reference Server (GMOD Project - select category 'DAS2'):
    -   <http://sourceforge.net/tracker/?group_id=27707&atid=391291>
-   Reference Client (IGB):
    -   <http://sourceforge.net/tracker/?group_id=129420&atid=714744>
-   Validation Suite (Dasypus):
    -   <https://sourceforge.net/tracker/?group_id=138271&atid=740641>

CVS Access
----------

There are separate repositories for the DAS/2 specification, DAS/2
client and server reference implementations, and the validation suite.
These are for developers only since they do not represent stable
releases and may contain modifications that contain bugs. The one
exception is the schema for the retrieval portion of the DAS/2 spec,
which is now stable.

-   DAS/2 spec:
    -   `:pserver:cvs@cvs.biodas.org:/home/repository/biodas`
        -   (login password="cvs", directory=das/das2, XML schema for
            retrieval spec: das2\_schemas.rnc)
-   [DAS/2 Client reference
    implementation](http://genoviz.sourceforge.net) ("IGB")
-   [DAS/2 Server reference implementation](http://gmod.org) (see
    das2 package)
-   [DAS/2 validation
    suite](http://sourceforge.net/projects/dasypus/) ("Dasypus")

See <http://www.open-bio.org/wiki/SourceCode> for general pointers about
anonymous access to [open-bio.org](http://open-bio.org)-hosted source
code.
