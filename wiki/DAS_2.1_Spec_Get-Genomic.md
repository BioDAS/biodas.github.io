---
title: DAS/2.1/Spec/Get-Genomic
permalink: wiki/DAS/2.1/Spec/Get-Genomic/
layout: wiki
---

**<big>Retrieving DAS2 genomic sequence and annotation feature
records</big>**

This document describes the DAS2 retrieval protocol for annotation
servers and genomic sequence reference servers. There are four main sets
of documents. The <b>sources</b> documents describe the available data
sources, the <b>segments</b> documents describe the reference genomic
sequences, the <b>features</b> documents contain the features on the
sequences, and the <b>types</b> documents characterize the different
feature types. The [formal schema for the XML
responses](http://biodas.org/documents/das2/das2_schemas.rnc) is a
RelaxNG schema in .rnc format.

Genome DAS/2 uses half-open intervals to specify regions along a
nucleotide sequence. See [Segment ranges](#Segment_ranges "wikilink")
for more information.

Every document is identified with a URI, which should be an HTTP URL. A
document is retrieved by doing a GET request on its URL. The following
table summarizes the various types of GET requests used within the
genome domain.

> <table border="1">
> <tr>
> <th>
> Request
>
> </th>
> <th>
> Documentation
>
> </th>
> <th>
> Information Retrieved
>
> </th>
> <th>
> Default Response Type
>
> </th>
> </tr>
> <tr>
> <td>
> sources
>
> </td>
> <td>
> [Sources Specification](/wiki/DAS/2/Spec/Get-Genomic/Sources "wikilink")
>
> </td>
> <td>
> Information about all data sources or a specific data source
>
> </td>
> <td>
> application/x-das-sources+xml
>
> </td>
> </tr>
> <tr>
> <td>
> segments
>
> </td>
> <td>
> ...
>
> </td>
> <td>
> Description of the regions on which features are located
>
> </td>
> <td>
> application/x-das-segments+xml
>
> </td>
> </tr>
> <tr>
> <td>
> types
>
> </td>
> <td>
> ...
>
> </td>
> <td>
> Description of all feature types or a single type
>
> </td>
> <td class="mono">
> application/x-das-types+xml
>
> </td>
> </tr>
> <tr>
> <td>
> features
>
> </td>
> <td>
> ...
>
> </td>
> <td>
> One or more genomic features, including sequence alignments
>
> </td>
> <td>
> application/x-das-features+xml
>
> </td>
> </tr>
> </table>

General Information
-------------------

This section contains information pertaining to the DAS/2 specification
as a whole.

### URIs, URLs and HTTP

This specification makes extensive use of URIs, and more specifically
HTTP URLs. While other URLs and URIs are possible the exchange protocol
uses concepts like request action and headers, response code and
headers, and query string construction which only make sense in the
context of HTTP and related protocols like HTTPS.

### Content-Type header

Each of the five new formats has its own MIME type. These are

  
application/x-das-sources+xml

application/x-das-features+xml

application/x-das-types+xml

A server should include the correct MIME type in the Content-Type header
of the response. If not it must respond with "application/xml" and must
not respond with text/xml. Character encoding is determined as per RFC
3023. We recommend that server implementers either not include the
charset parameter in the Content-Type header or ensure that it is
identical to the encoding in the document's XML declaration.

For use during specification development a server may include a
"version" value so clients can determine which version of the spec is
implemented by the server. Unless others can convince me otherwise this
will be removed in the final specification.

Example:

  
Content-Type: application/x-das-types+xml; version=300

The list of versions is as follows:

-   100 - the version as of 2006/02/07
-   200 - the version as of 2006/02/10
    -   (changed the feature query language format)
    -   (using "prop-" instead of "att" for property searches)
-   300 - the version as of 2006/03/10, which includes
    -   the updates from the first sprint. This document
    -   describes version 300.

If not present the client may assume the format is in the most recent
version.

### ISO dates

Several elements have 'created' and 'modified' attributes. These dates
are formatted in a subset of <a
href="http://www.w3.org/TR/NOTE-datetime">ISO 8601</a>. Data providers
must write the date using one of the following forms

-   Complete date:  
    YYYY-MM-DD (e.g. 1997-07-16)
-   Complete date plus hours and minutes:  
    YYYY-MM-DDThh:mmTZD (e.g. 1997-07-16T19:20+01:00)
-   Complete date plus hours, minutes and seconds:  
    YYYY-MM-DDThh:mm:ssTZD (eg 1997-07-16T19:20:30+01:00)  
    where:  
              YYYY = four-digit year
              MM   = two-digit month (01=January, etc.)
              DD   = two-digit day of month (01 through 31)
              hh   = two digits of hour (00 through 23) (am/pm NOT allowed)
              mm   = two digits of minute (00 through 59)
              ss   = two digits of second (00 through 59)
              TZD  = time zone designator (optional; one of the formats "Z", +hh:mm, +hhmm, -hh:mm, or -hhmm)
              

If the timezone designator is not specified a parser may assume 'Z'. If
the seconds are not specified then a parser may assume 0. If the time is
not specified then a parser may assume 12:00:00Z.

Here are some examples of valid dates

  
1970-08-22

<!-- -->

  
2005-06-30T13:08

1999-09-19T17:30Z

1995-12-25T07:00-07:00

1959-21-52T09:35+0300

<!-- -->

  
2000-01-01T01:23:45

2009-04-15T23:02:31Z

2001-10-22T21:39:12+01:00

2042-03-18T01:19:00-11:15

### Global sequence identifiers

Segments of genomic sequence referenced by a DAS/2 document are
identified by a set of community agreed upon identifiers. Global
sequence identifiers permit DAS/2 clients to merge annotations from
different servers if they both provide segments having the same name.

The canonical list of global sequence identifiers is publically
maintained and accessible via this wiki page:
<http://www.biodas.org/wiki/GlobalSeqIDs>.

### Segment ranges

Segment locations are used in three places in DAS: feature locations,
range-based feature filters, and sequence retrieval.

Every location is on a segment, named using its URI. This is either the
primary URI given in the segments document or the reference URI either
specified by the coordinate system or by the "reference" attribute in
the segments document.

Segment ranges are given by start and end positions specified using
half-open, zero-based intervals (a.k.a. interbase coordinates). The
interval is half-open meaning that the interval from 'start' to 'end'
includes the residues from position 'start' up to but not including
position 'end'. The first residue in the sequence is at position zero
and is specified as the range (0,1). The length of an interval is always
equal to end minus start (length=end-start).

This scheme is sometimes called "interbase coordinates" because the
numbering system labels the positions inbetween the bases (residues)
rather than the bases themselves.

For example, the range (3,6) includes the fourth, fifth, and sixth
residues not second or seventh:

          0   1   2   3   4   5   6   7
            G---A---T---C---C---G---A
                      [-----------]         
                        C---C---G     (range="3:6" from GATCCGA)

The end coordinate of a range is never less than the start position. The
range (5,6) covers the residue at position 5 while (5,5) has size of
zero and refers to the point between positions 4 and 5. Cleavage site
annotations may use zero size annotations like the latter.

Features may optionally be located on a strand. "1" denotes the positive
strand, "-1" denotes the negative strand, "0" denotes both strands. If
the strand is not specified then the strand is unknown or meaningless
for the given feature or sequence type.

Features ranges are specified using a compact notation. Given the start
and end positions the range is

        &lt;start&gt; + ":" + &lt;end&gt;

When the strand is allowed in a range field, it looks like:

        &lt;start&gt; + ":" + &lt;end&gt; + ":" + &lt;strand&gt;

Here are a few examples of how these look like in feature locations

-   all residues of Chr1  
           &lt;LOC segment="http://biodas.org/Chr1" /&gt;

-   the 1st and 2nd residues of Chr2  
           &lt;LOC segment="http://biodas.org/Chr2" range="0:2" /&gt;

-   the site between the 19th and 20th residues of Chr3  
           &lt;LOC segment="http://biodas.org/Chr3" range="20:20" /&gt;

-   the negative strand of Chr1 (assuming a length of 245522847)  
           &lt;LOC segment="http://ensembl.org/Homo_sapiens/Chr1" range="0:245522847:-1" /&gt;

Here are a few examples of how they look like in feature filters. Note
that feature filters do not support strand information in the query
range. The query parameters in both cases were encoded for use as a URL
query parameter.

-   All features on <http://ensemble.org/Homo_sapiens/Chr1> or Chr2
            http://www.biodas.org/das2/h.sapiens/v37/features?
              segment=http%3A%2F%2Fensembl.org%2FHomo_sapiens%2FChr1&amp;
              segment=http%3A%2F%2Fensembl.org%2FHomo_sapiens%2FChr2

-   All features that overlap residues 200-300 of Chr1
            http://www.biodas.org/das2/h.sapiens/v37/features?
              segment=http%3A%2F%2Fensembl.org%2FHomo_sapiens%2FChr1&amp;
              overlaps=200:300
        <pre></li>
        </ul>

        Sequence retrieval queries work directly on the segment URI.  To get
        the sequence for a subrange, pass the range using the "range" key of
        the query, as in the following:

        <ul>
          <li>The sequence for Contig4392
        <pre class="speclist">
          http://www.biodas.org/das2/h.sapiens/v37/sequence/Contig4392

-   The sequence for the first 10 residues of Contig4392 ("range=0:10")
          http://www.biodas.org/das2/h.sapiens/v37/sequence/Contig4392?range=0:10

### The link element

The LINK element connects a document or a feature record to some other
resource identified by a URL. The LINK element is modeled on the "link"
element in HTML 4.0 and has many of the same attributes, with identical
meanings.

The optional 'title' attribute is an advisory title providing a hint
about what the element links to. The optional 'href' attribute is the
URL being linked to. The optional 'type' attribute contains an advisory
MIME type describing the expected document content-type. For example, a
feature may link to an image in both GIF and a PNG formats, letting the
client chose a prefered format.

The 'rel' and 'rev' attributes contain space separated terms which
characterize the type of the forward and reverse links, respectively.
Likely most links will be forward links and so use the 'rel' attribute.
This specification does not reserve any link terms. We expect de facto
types to evolve through common use.

Here are a few examples

-   a segments, type, or feature record might refer to an "official"
    `    sources record:`  

        &lt;LINK rel="das2-sources" href="http://example.com/das2/human/v35/sources.xml"
          type="application/x-das-sources+xml" /&gt;

-   a feature filter response might link to an RSS feed. Subscribe
    `  to the feed to watch for any new results.`  

        &lt;LINK rel="alternate" title="RSS feed for this search
          type="application/rss+xml"
          href="http://example.com/das2/human/v35/rss?search_id=68302" /&gt;

-   a feature element may link to a bibliographic record
        &lt;LINK title="Primary reference" rel="pubmed-ref" 
          href="http://www.ncbi.nlm.nih.gov/entrez/query.fcgi?db=pubmed&cmd=Retrieve&list_uids=8744570" /&gt;

### Formats and extensibility

This specification defines a base set of XML document formats. A data
provider may want to return an alternate format in addition to the base
formats. For examples, if the XML overhead is too high a data provider
may want to develop a more compact binary feature format, or if the
feature data is too rich for the simple DAS property table then it would
be better represented in an alternate format.

Once implemented the alternative format support is announced through the
FORMAT elements of the CAPABILITY element.

For example, the following says that the server implements three
formats. The format name "das2xml" is reserved for the formats defined
by this specification and must be supported by the server even if not
listed. The other two format names are for example only:

    &lt;CAPABILITY type="features" query_uri="http://example.com/das/features.xml"&gt;
      &lt;FORMAT name="das2xml" /&gt;
      &lt;FORMAT name="das3xml" /&gt;
      &lt;FORMAT name="compact-binary" /&gt;
    &lt;/CAPABILITY&gt;

To request an alternate format the client must add a
"format=&lt;name&gt;" field to the query string of the URL. For example,
to request all of the features from the above example but in "das3xml"
the client makes a request for:

> ` `[`http://example.com/das/features.xml?format=das3xml`](http://example.com/das/features.xml?format=das3xml)

while to get all the features on <http://biodas.org/segment/Chr3> in
"compact-binary" format the client makes a request for

> ` `[`http://example.com/das/features.xml?segment=http%3A%2F%2Fbiodas.org%2Fsegment%2FChr3;format=compact-binary`](http://example.com/das/features.xml?segment=http%3A%2F%2Fbiodas.org%2Fsegment%2FChr3;format=compact-binary)

Servers may extend the <a href="#feature_filters">features filter
language</a> to add new capabilities as long as those terms do not
affect queries without those fields. A server may list support for a
query extension using the SUPPORTS tag. In the following the server says
it supports the "curation-search" as well as the das2xml and
compact-binary formats:

    &lt;CAPABILITY type="features" query_uri="http://example.com/das/features.xml"&gt;
      &lt;SUPPORTS name="curation-search" /&gt;
      &lt;FORMAT name="das2xml" /&gt;
      &lt;FORMAT name="compact-binary" /&gt;
    &lt;/CAPABILITY&gt;

The client implementer must use some other means to discover what
additional filters are available for a "curation-search".

A server may support additional capabilities not defined by this
specification and list support for it through a new CAPABILITY item. For
example, in the following the hypothetical server implements an
alternative query language based on XQuery.

    &lt;CAPABILITY type="xquery-features" query_uri="http://example.com/das-search" /&gt;

The contents of the non-DAS2 CAPABILITY elements is determined by the
server implementer and a client implementer must look elsewhere to
discover what it means.
