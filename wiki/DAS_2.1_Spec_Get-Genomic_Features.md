---
title: DAS/2.1/Spec/Get-Genomic/Features
permalink: wiki/DAS/2.1/Spec/Get-Genomic/Features/
layout: wiki
---

**<big>[DAS/2.1](/wiki/DAS/2.1/Spec/Get-Genomic "wikilink") Features Document
Specification for Genome Data Retrieval</big>**

Overview
--------

The versioned source record for an annotation server must include a
CAPABILITY element of type "features". Clients use the corresponding
query\_uri to get feature information from the server. By default
fetching this URL returns a list of all the features for the versioned
source as a document of content-type "application/x-das-features+xml". A
client may specify a set of <a href="#feature_filters">filters</a> to
retrieve a subset of the features and may ask for the response to be in
an alternative format. Servers may respond with an error if there are
too many matching features to return.

### Example request and response

Here is an example features document for a server which contains a gene
and an alignment.

Request:

> <http://das.biopackages.net/das/genome/yeast/S228C/features.pl>

Response:

    Content-Type: application/x-das-features+xml

    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;FEATURES xmlns="http://biodas.org/documents/das2"
              xml:base="http://www.example.org/volvox/1/"&gt;
     &lt;FEATURE uri="feature/cTel54X" type="type/gene" title="tg-3"&gt;
       &lt;LOC segment="segment/Chr2" range="1200:2917:1" /&gt;
     &lt;/FEATURE&gt;

     &lt;FEATURE uri="feature/hit12"
              type="type/est-alignment"
              created="2001-12-15T22:43:36"
              modified="2004-09-26T21:10:15" &gt;

       &lt;LOC segment="segment/Chr3" range="1201:1400:1" /&gt;
       &lt;PART uri="feature/hit12.hsp1" /&gt;
       &lt;PART uri="feature/hit12.hsp2" /&gt;
       &lt;PROP key="est2genomescore" value="180" /&gt;
     &lt;/FEATURE&gt;

     &lt;FEATURE uri="feature/hit12.hsp1"
              type="type/est-alignment-hsp"&gt;
       &lt;LOC segment="segment/Chr3" range="1201:1250:-1" /&gt;
       &lt;PARENT uri="feature/hit12"/&gt;
       &lt;PROP  key="est2genomescore" value="180" /&gt;
     &lt;/FEATURE&gt;

     &lt;FEATURE uri="feature/hit12.hsp2"
              type="type/est-alignment-hsp" &gt;
       &lt;LOC segment="segment/Chr3" range="1351:1400:1" /&gt;
       &lt;PARENT uri="feature/hit12" /&gt;
       &lt;PROP  key="est2genomescore" value="120" /&gt;
     &lt;/FEATURE&gt;

    &lt;/FEATURES&gt;

Each feature has a unique identifier and an identifer linking it to a
type record. Both identifiers are URLs and should be directly fetchable.
Simple features can be located on a region of a segment. More complex
features like a gapped alignment are represented through a parent/part
relationship. A feature may have multiple parents and multiple parts.

### Feature filters

An annotation server may contain many features while the client may only
be interested in a subset; most likely features in a given portion of
the reference sequence. To help minimize the bandwidth overhead the
feature query URL should support the DAS feature filter language. The
syntax uses the standard HTML
<a href="http://www.w3.org/TR/html4/interact/forms.html#h-17.13.4.1">form-urlencoded</a>
query syntax. Here is a request for all features on Chr2, assuming the
full URL for Chr2 is <http://example.org/volvox/1/segment/Chr2>

### Example request and response

Request:

> <http://www.example.org/volvox/1/features.cgi?segment=http%3A%2F%2Fexample.org%2Fvolvox%2F1%2Fsegment%2FChr2>

Response:

    Content-Type: application/x-das-features+xml

    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;FEATURES xmlns="http://biodas.org/documents/das2"
              xml:base="http://www.example.org/volvox/1/"&gt;
     &lt;FEATURE uri="feature/cTel54X" type="type/gene" title="tg-3"&gt;
       &lt;LOC segment="segment/Chr2" range="1200:2917:1" /&gt;
     &lt;/FEATURE&gt;

     &lt;FEATURE uri="feature/hit12"
              type="type/est-alignment"
              created="2001-12-15T22:43:36"
              modified="2004-09-26T21:10:15" &gt;

       &lt;LOC segment="segment/Chr3" range="1201:1400:1" /&gt;
       &lt;PART uri="feature/hit12.hsp1" /&gt;
       &lt;PART uri="feature/hit12.hsp2" /&gt;
       &lt;PROP key="est2genomescore" value="180" /&gt;
     &lt;/FEATURE&gt;
    &lt;/FEATURES&gt;

and here is the filter for all EST alignments, assuming those all have
the feature type '<http://www.example.org/volvox/1/type/est-alignment>'

### Example request and response

Request:

> <http://www.example.org/volvox/1/features.cgi?type=http%3A%2F%2Fwww.example.org%2Fvolvox%2F1%2Ftype%2Fest-alignment>

Response:

    Content-Type: application/x-das-features+xml

    &lt;FEATURES xmlns="http://biodas.org/documents/das2"
              xml:base="http://www.example.org/volvox/1/"&gt;
     &lt;FEATURE uri="feature/hit12"
              type="type/est-alignment"
              created="2001-12-15T22:43:36"
              modified="2004-09-26T21:10:15" &gt;

       &lt;LOC segment="segment/Chr3" range="1201:1400:1" /&gt;
       &lt;PART uri="feature/hit12.hsp1" /&gt;
       &lt;PART uri="feature/hit12.hsp2" /&gt;
       &lt;PROP key="est2genomescore" value="180" /&gt;
     &lt;/FEATURE&gt;
    &lt;/FEATURES&gt;

Details
-------

Each annotation server provides zero or more features as the result of a
feature query, which returns a features document. Each feature has a
title, a type, zero or more locations, and many other properties.

The feature query is done through the query\_uri of the "features"
CAPABILITY listed in the versioned source entry, as in the following:

    &lt;CAPABILITY type="features"
      query_uri="http://biodas.org/das/sequence/h_sapiens/Jun2006/feature-search.cgi" /&gt;

The response document is of content-type
"application/x-das-features+xml".

The CAPABILITY element may list alternative feature format using the
FORMAT element. The syntax details were discussed in the SOURCES
(detailed) section. The reserved feature format names are

<table border="1">
<tr>
<th>
format name

</th>
<th>
format description

</th>
</tr>
<tr>
<td>
das2xml

</td>
<td>
the XML format described here

</td>
</tr>
<tr>
<td>
count

</td>
<td>
the number of matching features

</td>
</tr>
<tr>
<td>
uris

</td>
<td>
a list of matching feature URIs

</td>
</tr>
</table>
All DAS servers must support the das2xml format. A client may assume the
server supports das2xml even if the format name is not listed in the
CAPABILITY.

The "count" format returns a document with a single line containing an
integer count of the number of feature elements that would be returned.
(A complex feature with a parent and a part has a count of 2 because it
has two FEATURE elements.) The content-type of this document should be
text/plain.

The "uris" format returns a document containing a newline separated list
of each feature URI matching the search. The content-type of this
document should be text/plain.

### CAPABILITY element

The CAPABILTY element may list the query interfaces supported by the
query\_uri using the SUPPORTS element. The syntax details were discussed
in the SOURCES (detailed) section. The reserved query names are

<table border="1">
<tr>
<th>
SUPPORTS name

</th>
<th>
query description

</th>
</tr>
<tr>
<td>
simple

</td>
<td>
a request may be made with no feature filters

</td>
</tr>
<tr>
<td>
das2queries

</td>
<td>
server implements the DAS2 <a href="#feature_filters">feature

` filter query language`</a>

</td>
</tr>
</table>
If only "simple" is listed then the server does not support the DAS2
<a href="#feature_filters">feature filter query language</a>. This
likely means that the server has at most a few thousand features and
it's easier for the client to download everything than to make multiple
requests against the server.

If "das2queries" is listed then the server implements the
<a href="#feature_filters">feature filter query language</a> defined in
this document. There is no extra meaning if "simple" is also listed.

If neither "simple" nor "das2queries" are given then a client may assume
the server supports "das2queries".

Here is an "features" CAPABILITY example using the FORMAT and SUPPORTS
elements:

    &lt;CAPABILITY type="features"
      query_uri="http://biodas.org/das/sequence/h_sapiens/Jun2006/feature-search.cgi"&gt;
      &lt;FORMAT name="das2xml" /&gt;
      &lt;FORMAT name="count" /&gt;
      &lt;SUPPORTS name="simple" /&gt;
      &lt;SUPPORTS name="das2queries" /&gt;
    &lt;/CAPABILITY&gt;

Fetching the query\_uri returns all of the features for the versioned
source. If the server considers the response too large then it MUST
respond with an HTTP error 413 "Request Entity Too Large", and the
message body SHOULD indicate what the server would consider an
acceptable response size. It is up to the server to determine how large
is "too large".

### Example request and response

Request:

> <http://biodas.org/das/sequence/fly/Jun2006/feature-search.cgi>

Response:

    Content-Type: application/x-das-features+xml

    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;FEATURES
         xmlns="http://biodas.org/documents/das2"
         xml:base="http://www.biodas.org/das2/sequence/fly/Jun2006/"&gt;

     &lt;FEATURE uri="./FT_9" type="./transposable_element" title="baggins"
        created="2005-11-24" doc_href="http://www.flybase.org/.bin/fbidq.html?FBgn0063440"&gt;
      &lt;ALIAS alias="CG8672" /&gt;
      &lt;ALIAS alias="CG3392" /&gt;
      &lt;ALIAS alias="CG8672" /&gt;
      &lt;ALIAS alias="baggins1" /&gt;
      &lt;ALIAS alias="Baggins1" /&gt;

      &lt;LOC segment="http://www.flybase.org/genome/D_melanogaster/R4.3/dna/4" range="637:1719:-1" /&gt;

      &lt;NOTE&gt;element type: non-LTR retrotransposon (Kapitonov and Jurka, 1997-*)&lt;/NOTE&gt;
      &lt;NOTE&gt;total length in bp: 5453 number of copies in genome: 14 in euchromatin of
    Release 3 genome annotation, of which zero are full length.&lt;/NOTE&gt;

      &lt;PROP key="gene_class" value="transposable element" /&gt;
     &lt;/FEATURE&gt;

     &lt;FEATURE uri="./FT_10" type="./exon" title="pseudogene CR32011"
        doc_href="http://www.flybase.org/.bin/fbidq.html?FBan0032011"&gt;
      &lt;ALIAS alias="FBtr0089182" /&gt;
      &lt;ALIAS alias="FBgn0076625" /&gt;
      &lt;LOC segment="http://www.flybase.org/genome/D_melanogaster/R4.3/dna/4" range="26993:27101" /&gt;
      
      &lt;NOTE&gt;pseudogene CR32011 (CR32011 ,FBgn0052011) is located on 4 and has a length
    of 5398 nt on the genomic sequence. The cytologic location is 101F1--102A1 . The
    physical map boundaries are 4:26,994..32,391[-].&lt;/NOTE&gt;
      &lt;NOTE&gt;Pseudogene related to CG17245&lt;/NOTE&gt;
      &lt;NOTE&gt;based on homology to the obsolete gene model for Dmel CG32011-PA amino acids
    1..1240 in release 3.1&lt;/NOTE&gt;

      &lt;PROP key="gene_region_length" value="5398" /&gt;
      &lt;fly:map xmlns:fly="http://flybase.org/"
               physical_map="4: 26,994..32,391[-]"
               cytogenetic_map="4: 101F1-102A1" /&gt;
     &lt;/FEATURE&gt;
    <!-- XXX The following is incorrect! How do I do alignments?
     &lt;FEATURE uri="feature/hit12"
              type="type/est-alignment"
              title="EST alignment"
              created="2001-12-15T22:43:36"
              modified="2004-09-26T21:10:15"
              doc_href="http://www.biodas.org/notes/est_alignment_note341.html"
              &gt;

       &lt;LOC segment="segment/Chr3" range="1201:1400:1" /&gt;
       &lt;PART uri="feature/hit12.hsp1" /&gt;
       &lt;PART uri="feature/hit12.hsp2" /&gt;
       &lt;PROP key="est2genomescore" value="180" /&gt;
     &lt;/FEATURE&gt;
    -->
    &lt;/FEATURES&gt;

</div>
### FEATURES element

The FEATURES element has zero or more <a href="#link_element">LINK
elements</a> linking the types document to some other document. The
content of the LINK attributes may depend on the
<a href="#feature_filters">filters</a> being used. For example, a LINK
element may point to an RSS feed which gives updates when new features
match the query. Different queries would point to different feeds.

The FEATURES element contains 0 or more FEATURE element. Each FEATURE
element has a 'uri' attribute which is a URI that must be unique for
each feature on the server. The URI should be fetchable and by default
return a document of content-type application/x-das-features+xml
containing the information for that feature. If the server supports the
"uris" FORMAT then each URI must be independently fetchable. If a
feature URI is fetchable then it must support the "format=" query
parameter, where the format name is given by "features" CAPABILITY of
the versioned source document and by the FORMAT list of the
corresponding feature type record.

The 'type' attribute of the FEATURE element is a URI identifying the
type record to which the feature belongs. The URI must be listed in the
full types document and should be individually fetchable.

The 'title' element contains a short description of the feature. This
should contain the feature name or other essential description of the
feature. The optional 'created' and 'modified' attributes contain
timestamps of when the the feature annotation was first created and most
recently modified. The optional 'doc\_href' is a URL for a web browser
to display human-readable documentation.

A FEATURE has zero or more ALIAS elements. Each ALIAS element lists an
alternate name for the feature, given by the 'alias' attribute.

A FEATURE has zero or more LOC elements describing where the annotation
is located on the segments. The 'segment' attribute of the LOC element
contains the URI of the segment on which the annotation is located. If
the versioned source document refers to "segments" capabilities then the
URI must be listed in one of the segments documents. Otherwise the URI
must match the URIs defined for the COORDINATE system stated in the
versioned source record.

### LOC element

A LOC record has an optional 'range' attribute. If not given the
location is on the entire segment. The range string is in one of the
following two formats:

        &lt;start&gt; + ":" + &lt;end&gt;
        &lt;start&gt; + ":" + &lt;end&gt; + ":" + &lt;strand&gt;

as for examples, "0:10", "30550:31625:-1". The ranges use the standard
DAS convention where the feature goes from the start position up to but
not including the end position, and the first position is "0".

A LOC record has an optional "gap" attribute which is a CIGAR string.
This can be used to specify details of a gapped alignment. The rationale
and syntax of CIGAR strings are explained in
<A href="http://www.genome.org/cgi/content/full/14/5/929">"The Ensembl
Core Software Libraries"</A>, Stabenau et. al., Genome Research 2004:
&lt;/&gt;

> An innovation in the storage of similarity search results is the
> compression of gapped alignment information in the form of dense
> character strings. This was originally developed as an output format
> from Exonerate (G. Slater, unpubl.) and is known by its original
> acronym "cigar" (concise idiosyncratic gapped alignment report). ...
> Each cigar line consists of an alternating series of numbers and
> letters, for example, 40M2I12M4D, with the letters standing for Match,
> Insertion, or Deletion. The number preceding each letter dictates the
> length of the match, insertion, or deletion; used together with the
> feature's start and end coordinates, the complete alignment can be
> reconstructed.

When a CIGAR string is used for the "gap" attribute in a LOC element,
the insertions and deletions specified in the CIGAR string MUST be
relative to the sequence indicated in that LOC's "segment" attribute.
For a pairwise alignment this usually corresponds to the "reference"
sequence used for the alignment, and the other LOC for the alignment
needs no "gap" attribute. But "gap" attributes can also be used to
exactly specify a multiple sequence alignment by giving a CIGAR string
for each LOC. &lt;/&gt;

A FEATURE has zero or more <a href="#link_element">LINK elements</a>
linking the feature record to an external database entry. This link
element is specific to the given feature. For example, it may link to an
image icon to be included in graphical displays.

Some features are complex and cannot easily be modeled with a single
feature record. Quoting from the "Chado Schema Documentation" (<a
href="http://www.gmod.org/?q=node/6"><http://www.gmod.org/schema></a>):

> `  The class of transplicing events that involve ligating transcripts`  
> `  from different loci into a mature mRNA requires a separate feature`  
> `  to represent each locus transcript and one to represent the fused`  
> `  transcript. The fragments are located on the fused transcript;`  
> `  portions of the fused transcript can also be located on the genome.`

Complex annotations are modeled through a directed acyclic graph
structure. Each node in the graph is a feature record, and is uniquely
labeled by its URI. A node may have zero or more children. A link from a
parent to a child indicates that the child is a "part of" the parent.
Cycles are not allowed - a node may not be a descendent of itself.
Complex features have a single root node. It may be a synthetic and
location-less feature if there is no natural physical interpretation for
it.

The FEATURE record stores the graph relationships in the PARENT and PART
elements. A FEATURE has zero or more PARENT elements. Each PARENT
element has a 'uri' attribute with the URI for a parent feature record.
A FEATURE has zero or more PART elements. Each PART element has a 'uri'
attribute with the URI for a child feature record. Having both parents
and parts listed means the graph structure is stored twice. In practice
this makes processing complex features easier than when only single
direction links are given.

A FEATURE has zero or more NOTE elements containing human-readable
information about the feature in the CDATA section of the element.
Clients and servers must treat the notes as an ordered list because the
text in a note may refer to another using terms like "previous note."

### The feature filter query language

Unless the "features" capability of the versioned source record only
lists support for "simple" searches, a DAS server must implement the
DAS2 query filter language. The language is based on a set of
predicates, specified as a key/value pair. If no predicates are given,
the feature query returns all features. Predicates act as filters and
reduce the number of features returned.

The query field keys are:

<table border="1">
<tr>
<th>
name

</th>
<th>
takes

</th>
<th>
matches features ...

</th>
</tr>
<tr>
<td>
link

</td>
<td>
URI

</td>
<td>
which have the given link

</td>
</tr>
<tr>
<td>
type

</td>
<td>
URI

</td>
<td>
with exactly the given type

</td>
</tr>
<tr>
<td>
segment

</td>
<td>
URI

</td>
<td>
on the given segment

</td>
</tr>
<tr>
<td>
coordinates

</td>
<td>
URI

</td>
<td>
which are part of the given coordinate system

</td>
</tr>
<tr>
<td>
overlaps

</td>
<td>
region

</td>
<td>
which overlap the given region

</td>
</tr>
<tr>
<td>
excludes

</td>
<td>
region

</td>
<td>
which have no overlap to the given region

</td>
</tr>
<tr>
<td>
inside

</td>
<td>
region

</td>
<td>
which are contained inside the given region

</td>
</tr>
<tr>
<td>
name

</td>
<td>
string

</td>
<td>
with a "title" or "alias" matching the given string

</td>
</tr>
<tr>
<td>
note

</td>
<td>
string

</td>
<td>
with a "note" matching the given string

</td>
</tr>
<tr>
<td>
prop-\*

</td>
<td>
string

</td>
<td>
with the property "\*" matching the given string

</td>
</tr>
</table>
Queries are
<a href="http://www.w3.org/TR/html4/interact/forms.html#h-17.13.4.1">form-urlencoded</a>
requests, meaning the DAS query is encoded in the query portion of an
HTTP GET URL. For example, if the feature query URL is
'<http://biodas.org/features>' and there is a segment named
'<http://ncbi.org/human/Chr1>' then the logical form of a request for
all features on the first 10,000 bases of that segment is

        segment = 'http://ncbi.org/human/Chr1'
        overlaps = 0:10000

which is form-urlencoded as

> ` `[`http://biodas.org/features?segment=http%3A%2F%2Fncbi.org%2Fhuman%2FChr1;overlaps=0:1000`](http://biodas.org/features?segment=http%3A%2F%2Fncbi.org%2Fhuman%2FChr1;overlaps=0:1000)

Multiple search terms with the same key are OR'ed together. The
following searches for features containing the name or alias of either
BC048328 or BC015400

> ` `[`http://biodas.org/features?name=BC048328;name=BC015400`](http://biodas.org/features?name=BC048328;name=BC015400)

The 'excludes' search is an exception. See below.

Multiple search terms with different keys are AND'ed together, but only
after doing the OR search for each set of search terms with identical
keys. The following searches for features which have a name or alias of
BC048328 or BC015400 and which are on the segment
<http://ncbi.org/human/Chr1>

> <http://biodas.org/features?name=BC048328;segment=http%3A%2F%2Fncbi.org%2Fhuman%2FChr1;name=BC015400>

The order of the search terms in the query string does not affect the
results.

If any part of a complex feature (that is, one with parents or parts)
matches a search term then all of the parents and parts are returned.

The fields which take URLs require exact matches, that is, a character
by character match. (For details on the nuances of comparing URIs see
<a href="http://www.textuality.com/tag/uri-comp-3.html">
<http://www.textuality.com/tag/uri-comp-3.html></a>).

The segment query filter takes a URI. This must accept the segment URI
and, if known to the server, the equivalent reference identifier for the
segment.

If range searches are given then one and only one segment must be given.
If there are multiple segment queries then ranges are not allowed.

The string searches may be exact matches, substring, prefix or suffix
searches. The query type depends on if the search value starts and/or
ends with a '\*'.

-   ABC -- field exactly matches "ABC"
-   -   ABC -- field ends with "ABC"
        </li>
-   ABC\* -- field starts with "ABC"
-   -   ABC\* -- field contains the substring "ABC"
        </li>

The interpretation of "\*" or "?" elsewhere in the query string is
implementation dependent. Text searches are case-insensitive. The string
"ABC" matches "abc", "aBc", "ABC", etc.

A server should collapse multiple whitespace characters into a single
space character for search purposes. For example, the query "\*a
newline\*" should match

      "This is a line of text which contains a
       newline"

The 'name' search does a text search of the 'title' and 'alias' fields.
A record matches if the name is found in any one of those fields.

The "prop-\*" is shorthand for a class of text searches of &lt;PROP&gt;
elements. Features may have properties, like

       &lt;PROP key="cellular_component" value="membrane" /&gt;

To do a string search for all 'membrane' cellular components, construct
the query key by taking the string "prop-" and appending the property
key text ("cellular\_component"). The query value is the text to search
for, in this case:

        prop-cellular_component=membrane

To search for any cellular\_component containing the substring
"membrane"

        prop-cellular_component=*membrane*

The rules for multiple searches with the same key also apply to the
prop-\* searches. To search for all 'membrane' or 'nuclear' cellular
components, use two 'prop-cellular\_component' terms, as

        http://biodas.org/features?prop-cellular_component=membrane;prop-cellular_component=membrane

The range searches are defined with explicit start and end coordinates.
The range syntax is in the form

      &lt;start&gt; + ":" + &lt;end&gt;

as for example, "1:9". There is no way to restrict the search to a
specific strand.

A feature may have several locations. An annotation may have several
features in a parent/part relationship. The relationship may have
several levels. If a range search matches any feature in the annotation
then the search returns all of the features in the annotation.

An 'overlaps' search matches if and only if any feature location of any
of the parent or part overlaps the query range and segment.

An 'inside' search matches if and only if at least one feature in the
annotation has a location on the query segment and all features which
have a location on the query segment have at least one location which
starts and ends in the query range.

EXPERIMENTAL: An 'excludes' matches if and only if at least one feature
of the annotation is on the query segment and no features are in the
query range. This is the complement of the 'overlaps' search, for
annotations on the same query segment.

Unlike the other search keys, if there multiple 'excludes' searches then
the results are AND'ed together. That is, if the query has two excludes
ranges

       segment=ChrX excludes=RANGE1 excludes=RANGE2

then the result are those features which on ChrX which are not in RANGE1
and are not in RANGE2.

### Additional feature properties

Some feature records may need to provide additional information.
Examples include alignment score information and curation history. There
are two ways to extend the basic type record.

The first is through a property table. The table is a list of zero or
more PROP elements. Each PROP element has a 'key' and 'value' attribute,
mapping the given given to value. Duplicate keys are allowed. There are
no pre-defined key names though de facto ones will likely arise through
use. The order of the PROP elements is arbitrary and must not affect the
meaning of the keys or values.

For more complex extensibility use non-DAS XML elements. Any number of
XML elements may occur after the property table so long as the outermost
elements are not in the DAS namespace. Clients must ignore unknown
elements.
