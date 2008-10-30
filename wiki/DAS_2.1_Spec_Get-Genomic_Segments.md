---
title: DAS/2.1/Spec/Get-Genomic/Segments
permalink: wiki/DAS/2.1/Spec/Get-Genomic/Segments/
layout: wiki
---

**<big>DAS/2 Segments Document Specification</big>**

-   **General specification:** [Retrieving DAS2 genomic sequence and
    annotation feature records](/wiki/DAS/2/Spec/Get-Genomic "wikilink")

Overview
--------

Features are located on segments. A segment is the largest chunk of
contiguous sequence. For fully sequenced organisms a segment may be a
chromosome. For partially assembled genomes where the distance between
the assembled regions is not known then each assembled region may be its
own segment. If a server provides annotations in contig space then each
contig is a segment. A specific set of segments is also called a
coordinate system.

There are two ways for a versioned source record to describe which
coordinate system it uses. The first is through a CAPABILITY element of
type "segments". (The CAPABILITY elements list the different DAS
interfaces and extensions supported by a server.) Fetching the
corresponding query\_uri returns a document of content-type
'application/x-das-segments+xml' listing information about each segment.

The second is through a COORDINATES element that uniquely characterizes
the coordinate system but requires consulting other sources for details
on the segments.

### Example request and response

Request:

> <http://www.biodas.org/das2/h.sapiens/v3/segments.xml>

Response:

    Content-Type: application/x-das-segments+xml

    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;das:SEGMENTS xmlns:das="http://biodas.org/documents/das2"&gt;
     &lt;das:SEGMENT uri="http://www.biodas.org/das2/h.sapiens/v37/segment/Chr1.xml"
         title="Chr1" length="245522847"
         doc_href="http://www.ensembl.org/Homo_sapiens/mapview?chr=1"/&gt;
     &lt;das:SEGMENT uri="http://www.biodas.org/das2/h.sapiens/v37/segment/Chr2.xml"
         title="Chr2" length="243018229"
         doc_href="http://www.ensembl.org/Homo_sapiens/mapview?chr=2"/&gt;
    &lt;/das:SEGMENTS&gt;

</div>
Note that unlike the previous examples this document defined the new
namespace abbreviation "das" instead of defining a default namespace.

Details
-------

A versioned source entry contains two ways to get information about the
top-level segments used as a coordinate system. One is through a
<a href="#global_seqids">global registry of COORDINATES URIs</a>. This
is an abstract identifier scheme. The other is through a concrete link
to a segments document, which lists information about each segment and
how to get the sequence information.

The segments request is done through the query\_uri of the "segments"
CAPABILITY listed in the versioned source entry, as in the following:

    &lt;CAPABILITY type="segments"
      query_uri="http://www.biodas.org/sequence/gallus_gallus/March2004.xml"&gt;

Fetching that URI returns a document of content-type
"application/x-das-segments+xml".

The versioned source may contain multiple segments CAPABILITIES. This
occurs when there are multiple top-level coordinate systems for the
annotation server, for example, with features annotated in both contig
and chromosome coordinates.

When that occurs each segments CAPABILITY must have a 'coordinates'
attribute containing a URI linking it to a COORDINATES element, which
must also exist in the versioned source. Note that the coordinates URIs
should but are not required to be in the global registry. You may make
up a URI if none exists . For example:

    &lt;COORDINATES uri="http://sanger.ac.uk/das-registry/yeast-32-gene"
               taxid="4932" source="Gene_ID" authority="SGD32" version="3"/&gt;
    &lt;COORDINATES uri="http://sanger.ac.uk/das-registry/yeast-32-chromosome"
               taxid="4932" source="Chromosome" authority="SGD32" version="3"/&gt;

    &lt;CAPABILITY type="segments"
      query_uri="http://www.biodas.org/sequence/s.cerevisiae/genes.xml"
      coordinates="http://sanger.ac.uk/das-registry/yeast-32-gene" /&gt;

    &lt;CAPABILITY type="segments"
      query_uri="http://www.biodas.org/sequence/s.cerevisiae/chromosomes.xml"
      coordinates="http://sanger.ac.uk/das-registry/yeast-32-chromosome" /&gt;

### Example request and response

Request:

> <http://www.biodas.org/sequence/gallus_gallus/March2004.xml>

Response:

    Content-Type: application/x-das-segments+xml

    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;SEGMENTS xmlns="http://biodas.org/documents/das2"
         xml:base="http://www.biodas.org/das2/sequence/fly/release4/"&gt;
     &lt;FORMAT name="fasta" /&gt;
     &lt;FORMAT name="agp" /&gt;
     &lt;SEGMENT uri="2L" title="Chromosome 2L" length="186349"
       reference="http://www.flybase.org/genome/D_melanogaster/R4.3/dna/2L" /&gt;
     &lt;SEGMENT uri="2R" title="Chromosome 2R" length="464030"
       reference="http://www.flybase.org/genome/D_melanogaster/R4.3/dna/2R" /&gt;
     &lt;SEGMENT uri="3L" title="Chromosome 3L" length="419684"
       reference="http://www.flybase.org/genome/D_melanogaster/R4.3/dna/3L" /&gt;
     &lt;SEGMENT uri="3R" title="Chromosome 3R" length="1428"
       reference="http://www.flybase.org/genome/D_melanogaster/R4.3/dna/3R" /&gt;
     &lt;SEGMENT uri="4" title="Chromosome 4" length="43776"
       reference="http://www.flybase.org/genome/D_melanogaster/R4.3/dna/4" /&gt;
     &lt;SEGMENT uri="X" title="Chromosome X" length="311673"
       reference="http://www.flybase.org/genome/D_melanogaster/R4.3/dna/X" /&gt;
    &lt;/SEGMENTS&gt;

Note that in the example the SEGMENT uri attributes are relative URIs
and are resolved using the xml:base defined in the SEGMENTS element.

### SEGMENTS element

The SEGMENTS element has zero or more FORMAT elements. A FORMAT element
has the single attribute named 'name' describing the supported format.
This specification defines the following format names:

<table border="1">
<tr>
<th>
Format name

</th>
<th>
Meaning

</th>
</tr>
<tr>
<td>
das2xml

</td>
<td>
a segments response of type application/x-das-segments+xml

</td>
</tr>
<tr>
<td>
fasta

</td>
<td>
sequence data in FASTA format

</td>
</tr>
<tr>
<td>
raw

</td>
<td>
sequence data with only residue names and newlines

</td>
</tr>
<tr>
<td>
agp

</td>
<td>
Assembly data in AGP format

</td>
</tr>
<tr>
<td>
count

</td>
<td>
the total number of segments, as a decimal string  
Used to get the segment count before potentially requesting

`      10,000s of segments`

</td>
</tr>
<tr>
<td>
formats

</td>
<td>
the standard das2xml format but without SEGMENT elements  
Used to get the format listing even when there are a large number

`    of segments`

</td>
</tr>
</table>
All versioned sources that have a "segments" capability must support the
"das2xml" format. The "das2xml" FORMAT entry does not need to be
specified in a FORMAT element. For details of use, see the next section.

### SEGMENT element

The SEGMENTS element has zero or more SEGMENT elements. Each SEGMENT
element has several attributes. The 'uri' attribute is a URI and must be
unique for each SEGMENT. The 'title' attribute is a short title of at
most a few words, meant for people. The 'length' attribute is an integer
count of the total number of residues in the segment. The optional
'doc\_href' is a URL for a web browser to display human-readable
documentation.

The optional 'reference' attribute is a URI. It connects the given
segment to the <a href="#global_seqids">globally agreed upon standard
identifier</a> for that segment. For example, the following reference
URI is the identifier for Human Chromosome 4, defined by NCBI assembly
34:

> `  `[`http://www.ncbi.nlm.nih.gov/genome/H_sapiens/B34.3/dna/chr4`](http://www.ncbi.nlm.nih.gov/genome/H_sapiens/B34.3/dna/chr4)` `

A client uses the reference identifier to merge features from different
DAS annotation servers into a common view because two segments from
different servers with the same reference identifier must be copies of
the same underlying segment.

### Segments query parameters

The segment URIs must be fetchable. This is used to return the sequence
data. Each segment URI must support
<i><a href="http://www.w3.org/TR/html4/interact/forms.html#h-17.13.4.1">form-urlencoded</a></i>
query parameters. The optional 'format' query specifies the data format
to return, using the values in the recently defined FORMAT elements. If
not given the default format is 'das2xml', which returns a segments
document of content-type 'application/x-das-segments+xml' containing the
details for only that segment. If the format query parameter is "fasta",
then the content-type for the returned FASTA document should be
"text/plain". FASTA records should have a title containing the segment
name but a client may ignore the title.

The segment URI queries support a 'range' query parameter to limit the
response to specific range of the segment. The query is of the form
"$start:$end", for examples "100:200" and "345:9876". Note that the
colon should be escaped for use in a URL.

The response must only include the sequence in the specified range. As
usual, the first residue is 0 and the range includes the start position
but not the end position. For example, if the sequence is "CATAGGTA"
then range=1:3 is the subsequence "AT".

Suppose the following is the segment URI for human chromosome 2

> ` `[`http://www.biodas.org/h.sapiens/v22/Chr2`](http://www.biodas.org/h.sapiens/v22/Chr2)

Fetching that URL will return a segment document for chromosome 2.
Fetching the URL with "format=fasta" in the query string, as in

> ` `[`http://www.biodas.org/h.sapiens/v22/Chr2?format=fasta`](http://www.biodas.org/h.sapiens/v22/Chr2?format=fasta)

returns the entire sequence for chromosome 2 in FASTA format. Adding
"range=500:900", as in

> ` `[`http://www.biodas.org/h.sapiens/v22/Chr2?format=fasta&range=500:900`](http://www.biodas.org/h.sapiens/v22/Chr2?format=fasta&range=500:900)

returns the 400 residues starting from the 501st position of chromosome
2, still in FASTA format.

If the server does not support a requested format name then it MUST
respond with an HTTP error message with status code 400 "Bad Request",
and the message body SHOULD indicate that the requested format is not
supported. If the server considers the response too large then it MUST
respond with an HTTP error message with status code 413 "Request Entity
Too Large", and the message body SHOULD indicate what the server would
consider an acceptable response size. It is up to the server to
determine how large is "too large".

If the start or end of the range is negative, or greater than the length
of the segment, or beyond the limits of the integer data type used for
the range then the server MAY respond with an HTTP error 400 "Bad
Request". Servers MUST accept ranges from 0 up to and including the
length of the segment, except for the case where the response is
considered too large
