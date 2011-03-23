---
title: DASWorkshop2011/day3/topics
permalink: wiki/DASWorkshop2011/day3/topics/
layout: wiki
---

Suggested Topics
================

Discussion about fitting alternative data formats into the DAS ecosystem (TAD)
------------------------------------------------------------------------------

See [proposal from 2010
workshop](/wiki/DAS1.6E#Support_for_alternative_content_formats "wikilink").

Also [Binary data support in
Dalliance](http://www.biodalliance.org/bin.html).

Security - authorization and authentication for clients, servers, registry (GS, AJ, JW)
---------------------------------------------------------------------------------------

Genome alignments in DAS (TAD)
------------------------------

[Some background](/wiki/DAS1.6E#Alignment "wikilink").

Briefly: there's a pretty good alignment data model, but the query
syntax currently has a few issues which limit its use for genomic
alignments. Most significantly, there's not a good way to fetch a subset
of the alignment based on *sequence* coordinates (e.g. "give me all
alignment blocks overlapping the segment 22:30000000,30100000 from human
NCBI36").

Improving interactivity of DAS data (TAD)
-----------------------------------------

Can we extend DAS to give better support for more interactive client
navigation? Things like next/previous feature buttons.

Customized html labels/pop ups? (JW)

Dasobert 2
----------

Improvements, Schema Driven? (JW, RJ))

Pagination in features response
-------------------------------

For search functionality (or in general?) (GS, AJ, RJ, JW, LG)

Philosophy of validation, spec compliance - client vs server expectations
-------------------------------------------------------------------------

Should the DAS registry say list all sources and just give information
on their validity rather than archiving sources. Should clients do all
they can to accommodate poor data sources or should they link to
validation data and encourage correct data source compliance?(JW)

Also: how aggressively should the registry check for and prune dead
datasources? Should there be some expectation of "maximum reasonable
response time" for a DAS request? (TAD)

Maxbins strategies
------------------

As simple as it can seem, binning strategies for the maxbins attribute
are not evident. Although the binning startegy is very dependant on data
type, it would be nice to have a small set of standard simple binning
algorithms so maxbins can be easily added to new DAS sources. (BG)
