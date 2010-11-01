---
title: DAS 1.6 Changes
permalink: wiki/DAS_1.6_Changes/
layout: wiki
---

Although it is the first in several years, the [DAS
1.6](/wiki/DAS1.6 "wikilink") specification is not principally intended to
accommodate brand new features. Instead, it broadly aims to:

1.  correct inconsistencies and ambiguities in the [DAS
    1.53](http://www.biodas.org/documents/spec-1.53.html) specification
2.  ratify concepts and extensions from [DAS
    1.53E](http://www.dasregistry.org/spec_1.53E.jsp) that are already
    in widespread use
3.  make more areas of the protocol easier to implement by relaxing
    unnecessary or unfeasible requirements
4.  deprecate legacy or little used elements of the protocol

As a result the changes are largely non-destructive and compatible. The
major exception to the above is the introduction of "parent/part"
hierarchical features. This is a replacement for the <GROUP> element,
inspired by [DAS/2](/wiki/DAS/2 "wikilink") rather than
[1.53E](http://www.dasregistry.org/spec_1.53E.jsp).

From the specification itself, the list of changes includes:

-   Introduced the concept of "coordinate systems" and the DAS Registry.
-   New commands: sources and structure. These commands are taken from
    DAS 1.53E, with some changes.
-   Added support for using ontologies in annotations.
-   Deprecated commands: dsn, dna and link.
-   Stylesheets now support histograms, colour gradients and line plots,
    more colours, and are better characterised. The "toomany" glyph
    is deprecated.
-   Entry points command gained "paging", and other minor changes.
-   Nonpositional annotations are now supported.
-   Features command gained "maxbins".
-   All command responses are now described by RELAX NG schemas.
-   Relaxed the constraints on data source names.
-   Clarified the use of HTTP and DAS status codes.
-   Clarified content encodings and compression.
-   Added requirement for clients to include specific request headers.
-   Features command gained hierarchical referencing, in favour
    of groups.
-   Segment query parameters no longer require start and end positions.
-   Several elements in the features command response are now optional.
-   Unified the format of segment XML across all commands.
-   Clarified the content of attributes across several commands, such as
    segment versions.

