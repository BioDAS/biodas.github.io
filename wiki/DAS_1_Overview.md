---
title: DAS/1/Overview
permalink: wiki/DAS/1/Overview/
layout: wiki
---

<big>**An Overview of the Concepts Concerning the Distributed Annotation
System (DAS/1)**</big>

NOTE: Some, but not all, concepts are applicable to the
[DAS/2](/wiki/DAS/2 "wikilink") version of the specification.

Information Source
------------------

The full specification, available at
<http://www.biodas.org/documents/spec.html>, serves as the primary
source for this document and will be plagiarized without explicit
notice. Any reference to this document will be made through the
convention <U>Specs</U>.

DAS Glossary
------------

(Definitions later defined are \_italicized\_; queries are \*bolded\*;
optional portions within names are \[bracketed\].)

### Distributed Annotation System (DAS)

A server system for the sharing of <I>Reference Sequences</I>, a
system<I> </I>conceptually composed of a <I>Reference Server </I>and
<I>Annotation Server(s)</I>.

### \[Reference\] Sequence

A sequence, consisting of a set of <I>entry points</I> into the sequence
and of the lengths of each entry point, which possesses a <I>reference
sequence ID</I>.

### \[Reference\] Sequence ID

The identification for a sequence, which corresponds to sequences of
either a low-level (<I>e.g.</I>, clones) or a high-level (<I>e.g.</I>,
contigs) and which is composed of any set of printable characters, save
for the colon, newline, tab, and carriage return characters.

### Entry point

A position defined for each genome at which the server may begin
dispensing data for a sequence, for a given length (of variable size),
<I>e.g.</I>, the head of a chromosome, the beginning of a series of
contigs, and the beginning of a contig. A list of entry points for a
given species may be retrieved via <B>entry\_points</B>.

### Annotation Server

A server specialized for returning lists of <I>annotations </I>across a
certain segment of the genome.

### Reference \[Sequence\] Server

An annotation server that, given a reference sequence ID, can also
return the following data:

1.  The raw DNA of the sequence;
2.  The annotations of the "component" of a <I>category
    </I>(<I>E.g.</I>, a contig is the component of a chromosome; thus,
    reference servers can return the annotation for a contig);
3.  The annotations of "supercomponents" of a <I>category
    </I>(<I>E.g.</I>, a chromosome is the supercomponent of a contig.).

### Annotation

An entity which:

1.  Is anchored to the genome map via a stop and start value relative to
    the reference subsequence;
2.  Possesses an ID unique to the server and a structured description of
    its nature and attributes;
3.  Optionally associated with Web URLs providing human-readable
    information about the annotation (via <B>link</B>);
4.  Possesses <I>types</I>, <I>methods</I>, and <I>categories</I>.

### \[Annotation\] Type

An entity selected from a list of types which have biological
significance and which roughly correspond to EMBL/GenBank feature table
tags, <I>e.g.</I>, exon, intron, CDS, and splice3.

### \[Annotation\] Method

A description of how the annotated feature was discovered, possibly
including a reference to a software program.

### \[Annotation\] Category

A intentionally broad functional genre that can be used to filter,
group, and sort annotations, <I>e.g.</I>, homology, variation, and
transcribed. (For the sake of consistency, <I>cf.</I> <U>Specs</U>:
"Feature Types and Categories" for a general list of types and
categories.)

### Source

A project containing data on DAS (a list of which may be retrieved via
<B>dsn</B>).

### Stylesheet

The server's non-binding recommendations on formatting retrieved
annotations for a given source (via <B>stylesheet</B>), using a General
Feature Format (GFF) document. (<I>Cf.</I>, <U>Specs</U>: "The Queries:
Retrieving the Stylesheet" and <U>Specs</U>: "Glyph Types.")

DAS Queries
-----------

A query can be made via a URL according to HTTP conventions, through
either GET or (more preferably because of size) POST. The response is
composed of:

1.  A standard HTTP header with DAS status information pertaining to the
    validity of the query (<I>cf.</I> <U>Specs</U>: "Client/Server
    Interactions: The Response.");
2.  (Optionally) an XML file containing the answer to the query,
    according to the specifications listed in <U>Specs</U>:
    "The Queries".

*PREFIX* denotes the URL prefix for the DAS server, e.g.,
<http://servlet.sanger.ac.uk:8080> is the prefix for
&lt;<http://servlet.sanger.ac.uk:8080/das/dsn%3E>. *DAS* denotes the
Data Source Name for a data source.

### dsn

> |            |                                                               |
> |------------|---------------------------------------------------------------|
> | *Command*  | *`PREFIX`*`/das/dsn`                                          |
> | *Function* | Retrieves the list of data sources available from this server |
> | *Scope*    | Reference and annotation servers                              |
>
### entry\_points

|            |                                                                                 |
|------------|---------------------------------------------------------------------------------|
| *Command*  | *`PREFIX`*`/das/`*`DSN`*`/entry_points`                                         |
| *Function* | Retrieves the list of entry points and their respective sizes for a data source |
| *Scope*    | Reference servers                                                               |

### dna

<I>Command</I>:
<I>PREFIX</I>/das/<I>DSN</I>/dna?segment=<I>RANGE</I>\[;segment=<I>RANGE</I>\]
<I>Function</I>: Retrieves the DNA associated with a subsequence

<I>Scope</I>: Reference servers

### sequence

<I>Command</I>:
<I>PREFIX</I>/das/<I>DSN</I>/sequence?segment=<I>RANGE</I>\[;segment=<I>RANGE</I>\]
<I>Function</I>: Retrieves the sequence associated with a subsequence

<I>Scope</I>: Reference servers

### types

<I>Command</I>:
<I>PREFIX</I>/das/<I>DSN</I>/types\[?segment=<I>RANGE</I>\]\[;segment=<I>RANGE</I>\]\[;type=<I>TYPE</I>\]

\[;type=<I>TYPE</I>\] <I>Function</I>: Retrieves the types available for
a segment of a sequence <I>Scope</I>: Reference and annotation servers

### features

<I>Command</I>:
<I>PREFIX</I>/das/<I>DSN</I>/features?segment=<I>REF:start,stop</I>\[;segment=<I>REF:start,stop</I>\]

\[;type=<I>TYPE</I>\]\[;type=<I>TYPE</I>\]\[;category=<I>CATEGORY</I>\]\[;category=<I>CATEGORY</I>\]
<I>Function</I>: Retrieves the annotations across a segment
<I>Scope</I>: Reference and annotation servers

### link

<I>Command</I>:
<I>PREFIX</I>/das/<I>DSN</I>/link?field=<I>TAG</I>;id=<I>ID</I>
<I>Function</I>: Retrieves and HTML page describing human-readable
information about an annotation <I>Scope</I>: Annotation servers

### stylesheet

<I>Command</I>: <I>PREFIX</I>/das/<I>DSN</I>/stylesheet <I>Function</I>:
Retrieves a stylesheet for the given <I>Scope</I>: Annotation servers

Genome Assembly
---------------

</B></FONT>

In a client application, Genome Assembly consists of moving "up" or
"down" (the nomenclature of the <U>Specs</U>, analogous to zooming "in"
or "out"), along component children and supercomponent parent(s)
<sup><A HREF="#1">1</A></sup>. Genome Assembly occurs only upon
Reference Servers, a necessary deduction from its definition. This data
is contained within the TYPE description for a feature. (<I>Cf.</I>
<U>Specs</U>: "Fetching Sequence Assemblies.)

Thus, in describing such a paradigm, the <U>Specs</U> appear to convey
that the client application will have to assemble information for a
given segment from its component children (<I>i.e.</I>, moving down).
(<I>E.g.</I>, a requested segment of a chromosome must be composed by
the assembly of several contigs.) Conversely, this paradigm simply
facilitates the client application to visit the supercomponent category
(<I>i.e.</I>, moving up). (<I>E.g.</I>, a user would like to zoom out
from a contig to view the entire chromosome.) However, the programmer
should note well that it is a logical possibility for a segment to span
more than one supercomponent parent (<I>e.g.</I>, a segment may span two
contigs).

<HR>
<A NAME="#1"></A>

<sup>1</sup> Following Lincoln Stein, I use the words <I>component</I>
and <I>supercomponent</I> to refer to categories alone. (<I>E.g.</I>,
the category contig is a component of the category chromosome, whereas
chromosome is a supercomponent of contig.) I shall use the words
<I>children</I> and <I>parent(s)</I> to refer to entities of the given
category. (<I>E.g.</I>, contigs 17, 18, 19, and 20 are the children of
the parent chromosome 4.)
