---
title: Spec Review
permalink: wiki/Spec_Review/
layout: wiki
---

**Note: this is NOT the official version of the [DAS
specification](http://biodas.org/documents/spec.html).**

Distributed Sequence Annotation System (DAS) Spec Review
========================================================

**Oct 1,2008**

This is a working document and a proposal for a reworked DAS
specification which hopes to clarify the DAS spec based on how DAS is
being used in the community today and to include commands from the
<a href="http://www.dasregistry.org/spec_1.53E.jsp">1.53E spec</a> and
some of the 2.0 spec. Also the document has been adjusted to reflect
changes in the use of DAS away from a solely genome centric protocol to
a more open one encompassing other reference/coordinate systems such as
alignments and protein structures. The spec also includes references to
the DAS Registry which is essential for implementing an SOA
architecture. Note: this is a technical document but should be readable
and understandable by people without a deep understanding of broader
technical issues and other system architectures.

Overview of the System
----------------------

This section provides a high-level view of the system architecture.

### System Components (Co-ordinate System Servers, Annotation Servers and the DAS Registry)

The DAS system consists of the a reference server, one or more
annotation servers and the das registry.

#### The Reference Server

Central to the DAS system is the idea of reference objects that can be
served from a reference server. These are biological data objects with
stable identifiers which are targets for annotation. In the original DAS
protocol, the reference objects are always biological sequences:
chromosomes, scaffold sequences from genome assemblies or protein
sequences. When a DAS client starts, its first action is to connect to
an appropriate reference server and retrieve the reference sequence.
Once a reference object has been loaded, the DAS client will contact one
or more annotation servers to obtain the data provided for the reference
objects. Typical annotations might include sets of predicted exons on a
genome sequence, or matches to a protein domain model on a protein
sequence. It is the client's responsibility to collect all relevant
annotations and display them in a user-friendly manner.

A coordinate system describes the data that is made available by a DAS
source/reference server. This information is important for the DAS
clients, to deal with data correctly, as they often can accept data
served in multiple coordinate systems. The coordinate systems are
described by four fields: Authority, (assembly) Version, Type, and
Organism. The assembly version is important for genome assemblies, but
not really applicable for other datasets like UniProt sequences,
therefore this field is optional.

#### Annotation Server

Annotation servers are specialized for returning lists of annotations
across a reference object served by the reference server. Each
annotation can be anchored to the co-ordinate system map by way of a
start and stop position relative to one of the reference
objects.Annotations have an ID that is unique to the server and a
structured description that describes its nature and attributes.
Annotations may also be associated with Web URLs that provide additional
human readable information about the annotation.

Annotations have <i>types</i>, <i>methods</i> and <i>categories</i>. The
annotation <b>type</b> is selected from a list of types that have
biological significance <font color="red"> though these do not help the
readability of xml returned so I can see why people are reluctant to
adapt them</font>), <font color="red">delete this EMBL bit?</font> and
correspond roughly to EMBL/GenBank feature table tags. Examples of
annotation types include "exon", "intron", "CDS" and "splice3."(We
encourage the use of the
<a href="http://www.sequenceontology.org/">Sequence Ontology</a>
<font color="blue">Tip: We recommend OboEdit for looking up ontologies
for regular use and the ontologies it uses can be downloaded from
<a href="http://www.berkeleybop.org/ontologies/#ontologies">here</a></font>

`IDs to give uniformity to DAS sources for example CDS is SO:0000316 `<font color="red">` is there a better resource out there that you can go from id to name and desciption and visa versa??`</font>`).  The annotation `<b>`method`</b>` is`

intended to describe how the annotated feature was discovered, and may
include a reference to a software program (we also encourage the use of
ECO numbers to represent the method of annotation e.g.ECO:0000032
"inferred from curated blast match to nucleic acid). The annotation

<a href="http://www.ebi.ac.uk/ontology-lookup/">

<b>category</b> is a broad functional category that can be used to
filter, group and sort annotations. "Homology", "variation" and
"transcribed" are all valid categories. The existence of these
categories allows researchers to add new annotation types if the
existing list is inadequate without entirely losing all semantic value.
The <a href="#categories">Annotation Categories</a> section contains a
list of the annotation types in use in the <cite>C. elegans</cite>
project.

It is intended that larger annotation servers provide pointers to
human-readable data that describes its types, methods and categories in
more detail. Another optional feature of annotation servers is the
ability to provide hints to clients on how the annotations should be
rendered visually. This is done by returning a XML "stylesheet".

The co-ordinate system server is an annotation server that provides the
following additional services:

1.  Given a reference sequence id, it can return the raw DNA of
    that sequence.
2.  Given a reference sequence id, it can return annotations of the
    `     category "component".  Component annotations describe how the`  
    `     sequence is assembled from smaller parts into large parts from the`  
    `     top down.`  
    ` `

3.  <font color="red">deprecate this?</font>Given a reference sequence
    id, it can return annotations of the
    `     category "supercomponent".  Component parent annotations`  
    `     describe the assembly of the sequence from the bottom up.`

Although the servers are conceptually divided between reference servers
and annotation servers, there is in fact no key difference between them.
A single server can provide both reference sequence information and
annotation information. The main functional difference is that the
reference sequence server is required to serve the sequence map and the
raw DNA, while annotation servers have no such requirement
<font color="red">how to generalise this? What does not serve sequence
info?</font>.

#### The DAS Registry

central DAS registry which implements this protocol and fulfils the
following roles:

1. It allows discovery of available DAS sources via a web page, or as
machine-readable XML which can be used directly by DAS client programs.

2. It automatically validates registered DAS sources to ensure that they
return well formed DAS XML.

3. It periodically tests DAS sources and notifies their administrators
if they are unavailable.

4. It can group the registered DAS sources according to the coordinate
systems of their data.

5. It can also communicate bi-directionally with DAS clients and
activate or highlight DAS sources in clients.

#### The Co-ordinate System

The distributed annotation system (DAS) relies on there being a common
"co-ordinate system" on which to base annotations. The co-ordinate
system consists of a set of "entry points", and the lengths of each
entry point <font color="red">but not all in the case of
alignments??</font>. The identity of an entry point will vary from
co-ordinate system to co-ordinate system. For some projects, entry
points correspond to entire chromosomes. For others, entry points may be
a series of contigs or proteins.

The entry points describe the top level items on the co-ordinate system.
<font color="red">remove this section on subsequence?</font>It is
possible for each entry point to have substructure, basically a series
of subsequences (components) and their start and end points. This
structure is recursive. Each annotation is unambiguously located by
providing its position as the start and stop positions relative to a
"reference object." The reference object can be one of the entry points,
or any of the subsequences within the entry point.  

Client/Server Interactions
--------------------------

The DAS is Web-based. Clients query the reference and annotation servers
by sending a formatted URL request to the server. This request must
follow the conventions of the HTTP/1.0 protocol (see
[RFC2616](ftp://ftp.isi.edu/in-notes/rfc2616.txt). Servers process the
request and return a response in the form of a formatted XML document
(see [W3C Extensible Markup Language](http://www.w3.org/XML/)).

### The Request

All DAS requests take the form of a URL. Each URL has a site-specific
prefix, followed by a standardized path and query string. The
standardized path begins with the string **/das**. This is followed by
URL components containing the data source name and a command. For
example:

<code>

[`http://www.wormbase.org/db/das/elegans/features?segment=CHROMOSOME_I:1000,2000`](http://www.wormbase.org/db/das/elegans/features?segment=CHROMOSOME_I:1000,2000)  
`^^^^^^^^^^^^^^^^^^^^^^^^^^ ^^^ ^^^^^^^ ^^^^^^^^ ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^`  
`   site-specific prefix    das  data   command  arguments`  
`                                 src `

</code>

In this case, the site-specific prefix is
**<http://www.wormbase.org/db>**. The request begins with the
standardized path **/das**, and the data source, in this case
**/elegans**. This is followed by the command **/features**, which
requests a list of features, and a query string providing named
arguments to the **/features** command.

The data source component allows a single server to provide information
on several genomes.

More information on the format of the request and the various available
commands is given \[\#commands below\].

The query string portion of the request (the "?" symbol rightward) can
be POSTed to the URL following conventional HTTP standards. Since some
queries can be quite large, this is the recommended way of argument
passing.

**<font color="red">Warning:</font>** The request may be replaced with a
SOAP-style XML-encapsulated document in future versions of this
specification.

### The Response

The response from the server to the client consists of a standard HTTP
header with DAS status information within that header followed
optionally by an XML file that contains the answer to the query. The DAS
status portion of the header consists of two lines. The first is
X-DAS-Version and gives the current protocol version number, currently
DAS/1.0. The second line is X-DAS-Status and contains a three digit
status code which indicates the outcome of the request.

Here is an example HTTP header: (*provided by Web server*)

<code>

`HTTP/1.1 200 OK                          `  
`Date: Sun, 12 Mar 2000 16:13:51 GMT          `  
`Server: Apache/1.3.6 (Unix) mod_perl/1.19    `  
`Last-Modified: Fri, 18 Feb 2000 20:57:52 GMT `  
`Connection: close                            `  
`Content-Type: text/plain                     `  
`X-DAS-Version: DAS/1.5`  
`X-DAS-Status: 200`  
`X-DAS-Capabilities: error-segment/1.0; unknown-segment/1.0; unknown-feature/1.0; ...`  
<em>`data follows...`</em>

</code>

The defined status codes are listed in Table 1.

| 200 | OK, data follows                                                |
|-----|-----------------------------------------------------------------|
| 400 | Bad command (command not recognized)                            |
| 401 | Bad data source (data source unknown)                           |
| 402 | Bad command arguments (arguments invalid)                       |
| 403 | Bad reference object (reference sequence unknown)               |
| 404 | Bad stylesheet (requested stylesheet unknown)                   |
| 405 | Coordinate error (sequence coordinate is out of bounds/invalid) |
| 500 | Server error, not otherwise specified                           |
| 501 | Unimplemented feature                                           |

The HTTP/1.0 protocol allows web clients to request byte-level
compression of the response by sending the HTTP header *Accept-Encoding*
header. Web servers that are capable of it can reply with a
*Content-transfer-encoding* header and a compressed body. Implementors
of DAS clients and servers may wish to implement this HTTP feature.

### New in version 1.5

The X-Das-Capabilities header provides an extensible list of the
capabilities that the server provides. This can be used by those writing
experimental extensions to DAS to flag clients that those extensions are
available. Capabilities have the form *CapabilityName/Version* and are
separated by semicolon, space, as in "capabilityA/1.0; capabilityB/1.4;
capabilityC/1.0". The following standard capabilities are present in the
DAS/1.5 protocol:

| Capability Name     | Description                                                                                                                                 |
|---------------------|---------------------------------------------------------------------------------------------------------------------------------------------|
| dsn/1.0             | The server supports the basic *dsn* request.                                                                                                |
| dna/1.0             | The server supports the basic *dna* request.                                                                                                |
| types/1.0           | The server supports the basic *types* request.                                                                                              |
| stylesheet/1.0      | The server supports the basic *stylesheet* request.                                                                                         |
| features/1.0        | The server supports the basic *features* request.                                                                                           |
| entry\_points/1.0   | The server supports the basic *entry\_points* request.                                                                                      |
| error-segment/1.0   | Server will report requests for invalid segments with an <ErrorSegment> response.                                                           |
| unknown-segment/1.0 | Server will report requests for unknown or unannotated segments with an <UnknownSegment> response.                                          |
| unknown-feature/1.0 | Server will report requests for unknown features with an <UnknownFeature> response.                                                         |
| feature-by-id/1.0   | The *features* request will accept the CGI parameter "feature\_id", enabling the server to look up segment(s) based on the ID of a feature. |
| group-by-id/1.0     | The *features* request will accept the CGI parameter "group\_id", enabling the server to look up segment(s) based on the ID of a group.     |
| component/1.0       | The *features* request will return components of the indicated segment when a category type of "component" is requested.                    |
| supercomponent/1.0  | The *features* request will return supercomponents of the indicated segment when a category type of "supercomponent" is requested.          |
| sequence/1.0        | The server supports the new *sequence* request.                                                                                             |

------------------------------------------------------------------------

Reference sequence IDs
----------------------

Reference sequence IDs indicate a segment of the genome. They can
correspond to low-level primary sequences such as sequenced clones, or
to higher-level assemblies such as contigs.

A reference ID can contain any set of printable characters (including
the space character), but not the colon character (":"), which is
reserved for separating reference IDs from sequence ranges (see below).
The newline, tab and carriage return characters are also reserved for
future use.

A data source that uses the colon character for its internal IDs must
map this character to another one on the way out and on the way in. For
example:

`   Client request       server's internal id         Response to client`  
  
`   gi-123456       -->  gi:123456                    --->  gi-123456`  
  
`   gi-123456:1,1000 --> gi:123456 start=1 stop=1000  --->  gi-123456:1,1000`  

------------------------------------------------------------------------

The Queries
-----------

This section lists the queries recognized by reference and annotation
servers. Each of these queries begins with some site-specific prefix,
denoted here as *PREFIX*. The other meta-variable used in these examples
is *DSN*, which is a symbolic data source. (As seen the the \[\#request
above example.\]) Data sources are standardized across DAS servers in
such a way that a data source name has a one-to-one correspondence with
a reference sequence.

### Retrieve the List of Data Sources

<font color="red">DSN command has been deprecated in favour of the
sources cmd</font>

**Scope:** Reference and annotation servers.

**Command:** *Sources*

**Format:**

<code>

*`PREFIX`*`/das/sources`

</code>

**Description:** This query returns the list of data sources that are
available from this server. In particular the following information for
a DAS server is important:

-   The email address of the maintainer of a DAS source
-   The <a href="help_coordsys.jsp">coordinate system</a>(namespace) of
    the provided data
-   different properties that allow to describe a server closer

  
<b>Scope:</b> all DAS servers

  
<b>Command:</b> <i>sources</i>

  
<b>Format:</b>


    <i>PREFIX</i>/das[1]/sources

The PREFIX can be either <b>das</b> or <b>das1</b> in order to refer to
the major version 1 of the DAS protocol and in order to provide support
for the future das2 protocol.  
  
<b>Description:</b> This query returns the meta information for a DAS
server  
<b>Arguments:</b> none  

<h4>
Response:

</h4>
  
The response to the <i>sources</i> command is the "DASSSOURCE"
XML-formatted document:

>     &lt;?xml version='1.0' encoding='UTF-8' ?&gt;
>     &lt;?xml-stylesheet type="text/xsl" href="das.xsl"?&gt;
>     &lt;SOURCES&gt;
>      &lt;SOURCE uri="URI" title="title" doc_href="URL" description="description"&gt;
>         &lt;MAINTAINER email="email address" /&gt;
>         &lt;VERSION uri="URI" created="date"&gt;
>
>           &lt;COORDINATES uri="uri" source="data type" authority="authority" test_range="ID">coordinate string&lt;/COORDINATES&gt;      
>           &lt;CAPABILITY type="das1:command" query_uri="URL" /&gt;
>           &lt;PROP name="key" value="value" /&gt;     
>          &lt;/VERSION&gt;
>        &lt;/SOURCE&gt;
>     &lt;/SOURCES>

  
<b>Format:</b>

<div id="table">
<table class="dastable">
<tr id="row2">
<td>
xml-stylesheet

</td>
<td>
optional

</td>
<td>
an XSL stylesheet that e.g. allows a browser to nicely display the XML
response

</td>
</tr>
<tr id="row1">
<td>
SOURCES

</td>
<td>
mandatory

</td>
<td>
the main container for several DAS sources

</td>
</tr>
<tr id="row2">
<td>
SOURCE

</td>
<td>
mandatory, one or many

</td>
<td>
the description for a DAS datasource

</td>
</tr>
<tr id="row1">
<td>
uri

</td>
<td>
mandatory

</td>
<td>
a unique URI for the DAS source

</td>
</tr>
<tr id="row2">
<td>
title, description

</td>
<td>
mandatory

</td>
<td>
the <i>nickname</i> under which a DAS server shall be known and
displayed in a view. The description is a free text description of the
provided data

</td>
</tr>
<tr id="row1">
<td>
doc\_href

</td>
<td>
optional

</td>
<td>
points to a web site where more information about a DAS source can get
obtained.

</td>
</tr>
<tr id="row2">
<td>
MAINTAINER, email

</td>
<td>
mandatory

</td>
<td>
the email address of the maintainer of this DAS source.

</td>
</tr>
<tr id="row1">
<td>
VERSION

</td>
<td>
mandatory

</td>
<td>
in principle this would allow hosting several versions of a DAS sources
(with unque uris) on a server, but in practise most people provide only
the server with the latest data. the created attribute provides the date
on which a DAS server has been set up initially. For a DAS registation
server this is the date at which a DAS server has been pulished.

</td>
</tr>
<tr id="row2">
<td>
COORDINATES

</td>
<td>
mandatory, one or many

</td>
<td>
The description of the namespace of a DAS source.  
<b>uri</b> - the unique URI for a DAS source. For a DAS registration
server these should be resolvable and allow to access more information
about this. e.g.
<a href="http://www.dasregistry.org/dasregistry/coordsys/CS_DS6"><http://www.dasregistry.org/dasregistry/coordsys/CS_DS6></a>
for the UniProt,Protein Sequence coordinate system.  
<b>source</b> - the data type. This refers to the "physical dimension"
of the data. Currently the following categories are available:
Chromosome, Clone, Contig, Gene\_ID, NT\_Contig, Protein Sequence,
Protein Structure  
<b>authority</b> - the authority, or institution that assigns the
accession code for this namespace. In case of genome assemblies the
authority that builds the assembly.

<b>version</b> - (optional) for genome assemblies the version of the
build.  
To learn more about coordinate systems, please see
<a href="help_coords.jsp">here</a>.

</td>
</tr>
<tr id="row1">
<td>
CAPABILTIY

</td>
<td>
mandatory, one or many

</td>
<td>
The supported DAS commmand  
<b>type</b> - the type of the DAS command. to distinguish DAS/1 from
DAS/2 servers <i>das1:</i> is used before the name of the command.  
<b>query\_uri</b>the URL of the server location, with the command
attached. e.g.
<http://www.ebi.ac.uk/das-srv/uniprot/das/uniprot/features> Note: For
some DAS commands this will not resolve, since e.g. for the features
command the extension

    /features?segment=ID

needs to be attached.

</td>
</tr>
<tr id="row2">
<td>
PROP

</td>
<td>
optional, one or many

</td>
<td>
a free key- value style property that allows to add more tags to a
server

</td>
</tr>
</table>
</div>
<b>Example Responses</b>

-   <a href="http://www.dasregistry.org/das1/sources"><http://www.dasregistry.org/das1/sources></a> -
    the listing of DAS/1 sources at the DAS registration server
-   <a href="http://www.ensembl.org/das/sources"><http://www.ensembl.org/das/sources></a> -
    the DAS sources provided by Ensembl. The DAS registration server
    uses this listing to automatically update the registration for these
    DAS servers.
-   <a href="http://vega.sanger.ac.uk/das/sources"><http://vega.sanger.ac.uk/das/sources></a> -
    the VEGA DAS sources

------------------------------------------------------------------------

### Retrieve the List of Entry Points for a Data Source

<font color="red">This Entry\_Points cmd is now mandatory for reference
servers.</font> **Scope:** Reference servers.

**Command:** *entry\_points*

**Format:**

<code>

*`PREFIX`*`/das/`*`DSN`*`/entry_points`  

</code>

**Description:** This query returns the list of sequence entry points
available and their sizes in base pairs.

**Arguments:**

**ref** (deprecated)  
If a sequence reference ID is provided in the **ref** argument, the
query will return the components of the sequence (its subsequences)
rather than the list of top-level entry point sequences. This argument
is **DEPRECATED**, and superseded by the "component" category of the
features request.

**type** (deprecated)  
For ACEDB servers, the **type** parameter provides the class of the
reference sequence, **Sequence** by default. **DEPRECATED**

#### Response:

The response to the *entry\_points* command is the "DASEP" XML-formatted
document:

**Format:**

<code>

<?xml version="1.0" standalone="no"?>
<!DOCTYPE DASEP SYSTEM "http://www.biodas.org/dtd/dasep.dtd">
<DASEP>  
`  `<ENTRY_POINTS href="''url''" version="''X.XX''">  
`    <SEGMENT id="`*`id1`*`" start="`*`start1`*`" stop="`*`stop1`*`"`<font color="red">` type="type" is now deprecated due to the registry should specify the type in this coordinate system `</font>` orientation="+">descriptive text`</SEGMENT>  
  
`    `<SEGMENT id="''id2''" start="''start2''" stop="''stop2''" type="type" orientation="+">`descriptive text`</SEGMENT>  
`    `<SEGMENT id="''id3''" start="''start3''" stop="''stop3''" type="type" orientation="+">`descriptive text`</SEGMENT>  
  
`    ...`  
`  `</ENTRY_POINTS>  
</DASEP>

</code>

;

<!DOCTYPE>
(required; one only)

  
The doctype indicates which formal DTD specification to use. For the
entry\_points query, the doctype DTD is
"<http://www.biodas.org/dtd/dasep.dtd>".

<DASEP> (required, one only)  
The appropriate doctype and root tag is DASEP.

<ENTRY_POINTS> (<font color="red">The start and stop are now optional</font>, only one)  
There is a single <ENTRY_POINTS> tag. It has a version number (required)
in the form "N.NN". Whenever the DNA of the entry point changes, the
version number should change as well. The **href** (required) attribute
echoes the URL query that was used to fetch the current document.

<SEGMENT> (optional; zero or more)  
Each segment contains the attributes **id**, **start**, **stop** and
**orientation**. The id is a unique identifier, which can be used as the
reference ID in further requests to DAS. The start and stop indicate the
start and stop positions of the segment. Orientation is one of "+" or
"-" and indicates the strandedness of the segment (use "+" if the
segment is not intrinsically ordered)<font color="red">The orientation
is now optional.</font>. If the optional **subparts** attribute is
present and has the value "yes", it indicates that the segment has
subparts.<font color="red">The subparts section is now
deprecated?</font><font color="red"> The type section is now deprecated
as this should be specified by the registry</font>.For compatibility
with older versions of the specification, the <SEGMENT> tag can use a
**size** attribute rather than **start** and **stop**, and can omit the
**orientation** attribute:

`      `<SEGMENT id="id" size="123456">  
`       In this case, the start is implied to be "1" and the stop is implied to be the same as the length.`

**Note:** The result from the entry points requests does not carry
sufficient information to reconstruct a complex sequence assembly.
Instead, use the features request with a category of "component". See
\[\#assemblies Fetching Sequence Assemblies\].

------------------------------------------------------------------------

### <font color="red">Retrieve the DNA Associated with a Subsequence has been deprecated as the sequence cmd below is more commonly used.</font>

------------------------------------------------------------------------

### Retrieve the Sequence Associated with a Subsequence

<b>Scope:</b> Reference servers.

<b>Command:</b> <i>sequence</i>

<b>Format:</b>

>     <i>PREFIX</i>/das/<i>DSN</i>/sequence?segment=<i>RANGE</i>[;segment=<i>RANGE</i>...]

<b>Description:</b> This query returns the sequence (nucleotide or
protein) corresponding to the indicated segment.

<b>Arguments:</b>

<dl>
<dt>
<b>segment</b> (required; one or more)

<dd>
This is the sequence range. It uses the format format
<i>reference:start,stop</i>, where

`     `<i>`reference`</i>` is the ID of the reference sequence used to establish the coordinate`  
`     system, and `<i>`start`</i>` and `<i>`stop`</i>` are the endpoints of the region to query, inclusive.`  
`     If the start and stop positions are not provided, then the entire reference sequence is`  
`     returned.`

</dl>
Here is an example of a valid request that uses the <b>segment</b>
argument to fetch three independent segments. The last segment is a
subsequence:

        http://www.wormbase.org/db/das/elegans/sequence?
            segment=BUM;segment=HUM_HGA;segment=CE_HOC2:1,200

<h4>
The Sequence Response</a>

</h4>
The response to <i>dna</i> is the "DASSEQUENCE" XML-formatted document.

<b>Format:</b>

>     &lt;?xml version="1.0" standalone="no"?&gt;
>     &lt;!DOCTYPE DASSEQUENCE SYSTEM "http://www.biodas.org/dtd/dassequence.dtd"&gt;
>     &lt;DASSEQUENCE&gt;
>
>       &lt;SEQUENCE id="<i>id</i>" start="<i>start</i>" stop="<i>stop</i>"
>                    moltype="<i>moltype</i>" version="X.XX"&gt;
>           atttcttggcgtaaataagagtctcaatgagactctcagaagaaaattgataaatattat
>           taatgatataataataatcttgttgatccgttctatctccagacgattttcctagtctcc
>           agtcgattttgcgctgaaaatgggatatttaatggaattgtttttgtttttattaataaa
>           taggaataaatttacgaaaatcacaaaattttcaataaaaaacaccaaaaaaaagagaaa
>           aaatgagaaaaatcgacgaaaatcggtataaaatcaaataaaaatagaaggaaaatattc
>           agctcgtaaacccacacgtgcggcacggtttcgtgggcggggcgtctctgccgggaaaat
>           tttgcgtttaaaaactcacatataggcatccaatggattttcggattttaaaaattaata
>           taaaatcagggaaatttttttaaattttttcacatcgatattcggtatcaggggcaaaat
>           tagagtcagaaacatatatttccccacaaactctactccccctttaaacaaagcaaagag
>           cgatactcattgcctgtagcctctatattatgccttatgggaatgcatttgattgtttcc
>           gcatattgtttacaaccatttatacaacatgtgacgtagacgcactgggcggttgtaaaa
>           cctgacagaaagaattggtcccgtcatctactttctgattttttggaaaatatgtacaat
>           gtcgtccagtattctattccttctcggcgatttggccaagttattcaaacacgtataaat
>           aaaaatcaataaagctaggaaaatattttcagccatcacaaagtttcgtcagccttgtta
>           tgtcaaccactttttatacaaattatataaccagaaatactattaaataagtatttgtat
>           gaaacaatgaacactattataacattttcagaaaatgtagtatttaagcgaaggtagtgc
>           acatcaaggccgtcaaacggaaaaatttttgcaagaatca
>       &lt;/SEQUENCE&gt;
>     &lt;/DASDNA&gt;

<dl>
<dt>
&lt;!DOCTYPE&gt; (required; one only)

<dd>
The doctype indicates which formal DTD specification to use.

`     For the sequence query, the doctype DTD is "`[`http://www.biodas.org/dtd/dassequence.dtd`](http://www.biodas.org/dtd/dassequence.dtd)`".`  
`     `

<dt>
&lt;DASSEQUENCE&gt; (required; one only)

<dd>
The appropriate doctype and root tag is DASSEQUENCE.

<dt>
&lt;SEQUENCE&gt; (required; one or more)

<dd>
There is a single &lt;SEQUENCES&gt; tag per requested segment. It has
the

`   attributes `<b>`id`</b>`, which indicates the reference ID for this sequence,`  
`     `<b>`start`</b>` and `<b>`stop`</b>`, which indicate the position of`  
`     this segment within the reference sequence, `<b>`moltype`</b>`,`  
`     which indicates the molecular type of the sequence, and `<b>`version`</b>`,`  
`     which provides the sequence map version number.  All five`  
`     attributes are required.`  
`     `

`     The molecule type is one of `<i>`DNA`</i>`, `<i>`ssRNA`</i>`,`  
`     `<i>`dsRNA`</i>`, or `<i>`Protein`</i>`.  No provision is made for`  
`     circular molecules.`  
`     `

`     The content of this tag is the sequence itself, using standard`  
`     IUPAC codes for DNA, RNA and protein.`

</dl>
</td>
</tr>
</table>
### Retrieve the Types Available for a Segment

**Scope:** Annotation and reference servers.

**Command:** *types*

**Format:**

<code>

*`PREFIX`*`/das/`*`DSN`*`/types [?segment=`*`RANGE`*`]`  
`                                   [;segment=`*`RANGE`*`]`  
`                                   [;type=`*`TYPE`*`]`  
`                                   [;type=`*`TYPE`*`]`  

</code>

**Description:** This query returns the annotation available for a
segment of sequence.

**Arguments:**

**segment** (optional)  
This is the sequence range. It uses the format format
*reference:start,stop*, where *reference* is the ID of the reference
sequence used to establish the coordinate system, and *start* and *stop*
are the endpoints of the region to query, inclusive.

**type** (optional)  
One or more type IDs to be used for filtering annotations on the
type field. If multiple type names are provided, the resulting list of
features will be the logical OR of the list. For compatibility with
versions 0.997 and earlier of this protocol, servers are allowed to
treat the type ID as a regular expression, but this feature is
**deprecated** and should not be used.

If one or more segment arguments are provided, the list of types
returned is restricted to the indicated segments. If no segment argument
is provided, then **all** feature types known to the source are
returned.

#### Response:

The document returned from the *types* request is an XML-formatted
"DASTYPES" documents. This is a shortened form of the full features
format (see below) and is used to summarize the type and number of each
annotation. Annotation types can be grouped into segments, or be totaled
across the entire database.

<code>

<?xml version="1.0" standalone="no"?>
<!DOCTYPE DASTYPES SYSTEM "http://www.biodas.org/dtd/dastypes.dtd">
<DASTYPES>  
`  `<GFF version="1.0" href="url">  
`  `<SEGMENT id="''id''" start="''start''" stop="''stop''" type="''type''" version="X.XX" label="''label''">  
  
`     `<TYPE id="''id1''" method="''method''" category="''category''">*`Type`
`Count` `1`*</TYPE>  
`     `<TYPE id="''id2''" method="''method''" category="''category''">*`Type`
`Count` `2`*</TYPE>  
  
`     ...`  
`  `</SEGMENT>  
`  `</GFF>  
</DASTYPES>

</code>

;

<!DOCTYPE>
(required; one only)

  
The doctype indicates which formal DTD specification to use. For the
types query, the doctype DTD is
"<http://www.biodas.org/dtd/dastypes.dtd>".

<DASTYPES> (required; one only)  
The appropriate doctype and root tag is DASTYPES.

<GFF> (required; one only)  
There is a single <GFF> tag. Its **version** (required) attribute
indicates the current version of the XML form of the General
Feature Format. The current version is (arbitrarily) 1.0. The
**href** (required) attribute echoes the URL query that was used to
fetch the current document.

<SEGMENT> (required; one or more)  
The <SEGMENT> tag provides information on the reference segment. The
**id**, **start** and **stop** attributes indicate the coordinate system
of the segment, and are required if the segment corresponds to a defined
region of the genome, optional if the list of types corresponds to the
entire database. The **version** attribute (required) indicates the
current version of the sequence map. The optional **label** attribute
supplies a human readable label for display purposes. The optional type
attribute describes the segment type, for future compatibility with
Sequence Ontology-based feature typing.

<TYPE> (optional; zero or more per SEGMENT)  
Each segment has zero or more <TYPE> tags, which summarize the types of
annotation available. The attributes are **id** (required), which is a
unique id for the annotation type and can be used to retrieve further
information from the annotation server (see \[\#feature\_linking Linking
to a Feature\]), **method** (optional), which indicates the
method (subtype) for the feature type and the **category** (optional)
attribute, which provides functional grouping to related types. The tag
contents (optional) is a count of the number of features of this type
across the segment.

------------------------------------------------------------------------

### Retrieve the Annotations Across a Segment

**Scope:** Reference and annotation servers.

**Command:** *features*

**Format:**

<code>

*`PREFIX`*`/das/`*`DSN`*`/features?segment=`*`REF:start,stop`*`[;segment=`*`REF:start,stop`*`...]`  
`                                      [;type=`*`TYPE`*`]`  
`                                      [;type=`*`TYPE`*`]`  
`                                      [;category=`*`CATEGORY`*`]`  
`                                      [;category=`*`CATEGORY`*`]`  
`                                      [;categorize=`*`yes|no`*`]`  
`                                      `<font style="background-color: #DEB887">`[;feature_id=ID]`</font>  
  
`                                      `<font style="background-color: #DEB887">`[;group_id=ID]`</font>

</code>

**Description:** This query returns the annotations across one or more
segments of sequence.

**Arguments:**

**segment** (<font style="background-color: #DEB887">zero</font> or more)  
If specified, the segment argument restricts the list of annotations to
those that overlap the indicated range. Each segment argument uses the
format format *reference:start,stop*, where *reference* is the ID of the
reference sequence used to establish the coordinate system, and *start*
and *stop* are the endpoints of the region to query, inclusive. Multiple
segments may be specified.

**type** (zero or more)  
Zero or more type IDs to be used for filtering annotations on the
type field. If multiple type names are provided, the resulting list of
features will be the logical OR of the list. For compatibility with
versions 0.997 and earlier of this protocol, servers are allowed to
treat the type ID as a regular expression, but this feature is
**deprecated** and should not be relied on.

**category** (zero or more)  
Zero or more category IDs to be used for filtering annotations
by category. If multiple categories are provided, they are treated as
the logical OR. For compatibility with versions 0.997 and earlier of
this protocol, servers are allowed to treat the type ID as a regular
expression, but this feature is **deprecated** and should not be
relied on.

**categorize** (optional)  
Either "yes" or "no" (default). If "yes", then each annotation must
include its functional category.

**feature\_id**<font style="background-color: #DEB887"> (zero or more; new in 1.5)</font>  
Instead of, or in addition to, **segment** arguments, you may provide
one or more **feature\_id** arguments, whose values are the identifiers
of particular features. If the server supports this operation, it will
translate the feature ID into the segment(s) that strictly enclose them
and return the result in the *features* response. It is possible for the
server to return multiple segments if the requested feature is present
in multiple locations.

**group\_id**<font style="background-color: #DEB887"> (zero or more; new in 1.5)</font>  
The **group\_id** argument, is similar to **feature\_id**, but retrieves
segments that contain the indicated feature group.

Annotation servers are only required to return annotations which are
completely contained within the indicated segment. Servers **may** also
return annotations which overlap the segment, but are not completely
contained within them. Annotations **must** be returned using the
coordinate system in which they were requested. For example, if a contig
ID was used to specify the segment, then the annotation endpoints must
use contig coordinates.

If multiple segment arguments are provided and they happen to overlap,
then the annotation server may return the same annotation multiple
times, possibly using different coordinate systems. It is the
responsibility of the client to merge annotations based on the assembly.

#### Response:

The document returned from the *features* request is an XML-formatted
"DASGFF" document.

**Format:**

<code>

<?xml version="1.0" standalone="no"?>
<!DOCTYPE DASGFF SYSTEM "http://www.biodas.org/dtd/dasgff.dtd">
<DASGFF>  
`  `<GFF version="1.0" href="url">  
`  `<SEGMENT id="''id''" start="''start''" stop="''stop''" type="''type''" version="X.XX" label="''label''">  
  
`      `<FEATURE id="''id''" label="''label''">  
`         `<TYPE id="''id''" category="''category''" reference="''yes|no''">*`type`
`label`*</TYPE>  
  
`         `<METHOD id="''id''">`'' method label ''`</METHOD>  
`         `<START>`'' start'' `</START>  
`         `<END>`'' end'' `</END>  
  
`         `<SCORE>`'' [X.XX|-]'' `</SCORE>  
`         `<ORIENTATION>` [0|-|+] `</ORIENTATION>  
`         `<PHASE>` [0|1|2|-]`</PHASE>  
  
`         `

<NOTE>
*note text*

</NOTE>
`    `<LINK href="''url''">` `*`link` `text`*` `</LINK>  
`    `<TARGET id="id" start="x" stop="y">*`target` `name`*</TARGET>  
  
`    `<GROUP id="''id''" label="''label''" type="''type''">  
`          `

<NOTE>
*note text*

</NOTE>
`          `<LINK href="''url''">` `*`link` `text`*` `</LINK>  
  
`          `<TARGET id="id" start="x" stop="y">*`target`
`name`*</TARGET>  
`         `</GROUP>  
`      `</FEATURE>  
`      ...`  
`  `</SEGMENT>  
`  `</GFF>  
  
</DASGFF>

</code>

;

<!DOCTYPE>
(required; one only)

  
The doctype indicates which formal DTD specification to use. For the
features query, the doctype DTD is
"<http://www.biodas.org/dtd/dasgff.dtd>".

<DASGFF> (required; one only)  
The appropriate doctype and root tag is DASGFF.

<GFF> (required; one only)  
There is a single <GFF> tag. Its **version** (required) attribute
indicates the current version of the XML form of the General
Feature Format. The current version is (arbitrarily) 1.0 The
**href** (required) attribute echoes the URL query that was used to
fetch the current document.

<SEGMENT> (required; one or more)  
The <SEGMENT> tag provides information on the reference segment
coordinate system. The **id**, **start** and **stop** attributes
indicate the position of the segment. The **version** attribute
indicates the current version of the sequence map. The id, start, stop,
and version attributes are required. The optional **label** attribute
provides a human readable label for display purposes. The optional type
attribute describes the segment type, for future compatibility with
Sequence Ontology-based feature typing.

<FEATURE> (optional; zero or more per SEGMENT)  
There are zero or more <FEATURE> tags per <SEGMENT>, each providing
information on one annotation. The **id** attribute (required) is a
unique identifier for the feature. It can be used as a reference point
for further navigation. The **label** attribute (optional) is a
suggested label to display for the feature. If not present, the **id**
attribute can be used instead.

<TYPE> (required; one per FEATURE)  
Each feature has just one <TYPE> field, which indicates the type of
the annotation. The attributes are **id** (required), which is a unique
id for the annotation type and can be used to retrieve further
information from the annotation server (see \[\#feature\_linking Linking
to a Feature\]), and the **category** (optional, recommended) attribute,
which provides functional grouping to related types. The reference
server's annotations can consist of additional overlapping landmarks
(parents, children, and neighbors), which should be marked "yes" in the
third attribute **reference** (optional, defaults to "no") to indicate
that the feature is a structural landmark within the map (this feature
can be annotated). The tag contents (optional) is a human readable label
for display purposes.If a **reference** annotation has either or both of
the optional attributes, **subparts="yes"** and **superparts="yes"**,
then in addition to being useable as a reference sequence, the feature
contains subparts and/or superparts that themselves can act as
reference features. This can be used to reconstruct reference
server's assembly. See also \[\#fetching\_assembly Fetching
Assembly Information\].

<METHOD> (required; one per FEATURE)  
Each feature has one <METHOD> field, which identifies the method used to
identify the feature. The **id** (optional) tag can be used to retrieve
further information from the annotation server. The tag
contents (optional) is a human readable label.

<START>, <END> (required; one apiece per FEATURE)  
These tags indicate the start and end of the feature in the coordinate
system of the reference sequence given in the <SEGMENT> tag. The
relationship between the feature start and stop positions and the
segment start and stop is that the two spans are guaranteed to overlap.

<SCORE> (required; one per FEATURE)  
This is a floating point number indicating the "score" of the method
used to find the current feature. The number can only be understood in
the context of information retrieved from the server by linking to
the method. If this field is inapplicable, the contents of the tag can
be replaced with a **-** symbol.

<ORIENTATION> (required; one per FEATURE)  
This tag indicates the orientation of the feature relative to the
direction of transcription. It may be for features that are unrelated to
transcription, **+**, for features that are on the sense strand, and
**-**, for features on the antisense strand.

<PHASE> (required; one per FEATURE)  
This tag indicates the position of the feature relative to open reading
frame, if any. It may be one of the integers , **1** or **2**,
corresponding to each of the three reading frames, or **-** if the
feature is unrelated to a reading frame.

;

<NOTE>
(optional; zero or more per FEATURE)

  
A human-readable note in plain text format.

<LINK> (optional; zero or more per FEATURE)  
A link to a web page somewhere that provides more information about
this feature. The **href** (required) attribute provides the URL target
for the link. The link text is an optional human readable label for
display purposes.

<TARGET> (optional; zero or more per FEATURE)  
The target sequence in a sequence similarity match. The **id** attribute
provides the reference ID for the target sequence, and the **start** and
**stop** attributes indicate the segment that matched across the
target sequence. All three attributes are required. More information on
the target can be retrieved by linking back to the annotation server.
See \[\#feature\_linking Linking to a Feature\].

<GROUP> (optional; zero or more per FEATURE)  
The <GROUP> section is slightly odd, as it is derived from an overloaded
field in the GFF flat file format. It provides a unique "group" ID that
indicates when certain features are related to each other. The canonical
example is the CDS, exons and introns of a transcribed gene, which
logically belong together. The group **id** attribute (required)
provides an identifier that should be used by the client to group
features together visually. Unlike other IDs in this protocol, the group
ID cannot be used as a database handle to retrieve further information
about the group. Such information can, however, be provided within
<GROUP> section, which may contain up to three optional tags.The
**label** attribute (optional) provides a human-readable string that can
be used in graphical representations to label the glyph.The **type**
attribute (optional) provides a type ID for the group as a whole, for
example "transcript". This ID can be used as a key into the
\[\#stylesheet stylesheet\] to select the glyph and graphical
characteristics for the group as a whole.

;;

<NOTE>
(optional; zero or more per GROUP)

  
  
A human-readable note in plain text format.

; <LINK> (optional; zero or more per GROUP)  
  
A link to a web page somewhere that provides more information about
this group. The **href** (required) attribute provides the URL target
for the link. The link text is an optional human readable label for
display purposes.

; <TARGET> (optional; zero or more per GROUP)  
  
The target sequence in a sequence similarity match. The **id** attribute
provides the reference ID for the target sequence, and the **start** and
**stop** attributes indicate the segment that matched across the
target sequence. All three attributes are required. NOTE: although this
tag is present in the GROUP section, it applies to the FEATURE, and it
is preferred to place it directly in the <FEATURE> section. Earlier
versions of this specification placed the TARGET tag in the GROUP
section, and clients must recognize and accomodate this.

| New in version 1.5: Exception Handling for Invalid Segments                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| The request for a named segment may fail because: (1) the reference sequence is not known to the server or (2) the requested region is outside the bounds of the segment. In both cases, an exception is indicated. In the case of a reference server, which is expected to be authoritative for the map, the <GFF> section will flag the problem by issuing an <ERRORSEGMENT> tag instead of the usual <SEGMENT> tag. This tag has the following format:<ERRORSEGMENT id="id" start="start" stop="stop">The **id** attribute (required) corresponds to the ID of the requested segment, and **start** and **stop** (optional) correspond to the requested bounds of the segment if this was specified in the request.Unlike a reference server, an annotation server is not required to know the identities of all the segments. Therefore when presented with a segment ID that it doesn't recognize, it can't know whether this is a true client error or merely an unannotated segment. In this case, an annotation server will issue an <UNKNOWNSEGMENT> tag. This tag has the same syntax as <ERRORSEGMENT> but doesn't necessarily imply an error.If an annotation server detects a request for a region outside the bounds of a segment that it has annotated, it will issue an <ERRORSEGMENT> exception.In the case of a request for multiple segments, the server will return a mixture of <SEGMENT> sections for valid segments, and <ERRORSEGMENT> or <UNKNOWNSEGMENT> sections for invalid ones. |

------------------------------------------------------------------------

### Linking to a Feature

**Scope:** Annotation servers.

**Command:** *link*

**Format:**

<code>

*`PREFIX`*`/das/`*`DSN`*`/link?field=`*`TAG`*`;id=`*`ID`*

</code>

**Description:** This query can be issued in order to retrieve further
human-readable information about an annotation. It is best to pass this
URL directly to a browser, as the type of the returned data is not
specified (it will typically be an HTML file, but any MIME format is
allowed).

**Arguments:**

**field** (required)  
The field to fetch further information on. Options are:

-   **feature** -- the feature itself
-   **type** -- the feature type
-   **method** -- the feature method
-   **category** -- the feature category
-   **target** -- the target, applicable to sequence similarities only

**id** (required)  
The ID of the indicated annotation field.

**Response:** A web page.

------------------------------------------------------------------------

### Retrieving the Stylesheet

**Scope:** Annotation servers.

**Command:** *stylesheet*

**Format:**

<code>

*`PREFIX`*`/das/`*`DSN`*`/stylesheet`

</code>

**Description:** This query can be issued to an annotation server in
order to retrieve the server's recommendations on formatting annotations
retrieved from it. These recommendations are not normative. A viewer is
free to use any display format it chooses.

**Arguments:** None.

#### Response:

This document is intended to provide hints to the annotation display
client. It maps feature categories and individual types to a series of
glyphs known to the display client.

**Format:**

<code>

<?xml version="1.0" standalone="no"?>
<!DOCTYPE DASSTYLE SYSTEM "http://www.biodas.org/dtd/dasstyle.dtd">
<DASSTYLE>  
`  `<STYLESHEET version="X.XX">  
  
`     `<CATEGORY id="default">  
`         `<TYPE id="default">  
`      `<GLYPH zoom="high">  
  
`             <`*`ID`*`>`  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
`          ...`  
`             </`*`ID`*`>`  
  
`      `</GLYPH>  
`      `<GLYPH zoom="medium">  
`             <`*`ID`*`>`  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
  
`          ...`  
`             </`*`ID`*`>`  
`      `</GLYPH>  
`      `<GLYPH zoom="low">  
`             <`*`ID`*`>`  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
`          ...`  
`             </`*`ID`*`>`  
`      `</GLYPH>  
`         `</TYPE>  
`     `</CATEGORY>  
  
`     `<CATEGORY id="group">  
`         `<TYPE id="group_id1">  
`      `<GLYPH zoom="high">  
`             <`*`ID`*`>`  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
`          ...`  
`             </`*`ID`*`>`  
`      `</GLYPH>  
`           ...`  
`     `</CATEGORY>  
  
  
`     `<CATEGORY id="''category1''">  
`         `<TYPE id="''default''">  
`      `<GLYPH>  
`             <`*`ID`*`>`  
  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
`          ...`  
`             </`*`ID`*`>`  
`      `</GLYPH>  
`         `</TYPE>  
`         `<TYPE id="''type1''">  
  
`      `<GLYPH>  
`             <`*`ID`*`>`  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
`          ...`  
`             </`*`ID`*`>`  
`      `</GLYPH>  
  
`         `</TYPE>  
`         `<TYPE id="''type2''">  
`      `<GLYPH>  
`             <`*`ID`*`>`  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
  
`          ...`  
`             </`*`ID`*`>`  
`      `</GLYPH>  
`         `</TYPE>  
`         ...`  
`     `</CATEGORY>  
  
`     `<CATEGORY id="''category2''">  
  
`         `<TYPE id="''default''">  
`      `<GLYPH>  
`             <`*`ID`*`>`  
`          <`*`ATTR`*`>`*`value`*</ATTR>  
`          ...`  
`             </`*`ID`*`>`  
  
`      `</GLYPH>  
`   `</TYPE>  
`         ...`  
`     `</CATEGORY>  
`     ...`  
  
</STYLESHEET>  
</DASSTYLE>

</code>

;

<!DOCTYPE>
(required; one only)

  
The doctype indicates which formal DTD specification to use. For the
stylesheet query, the doctype DTD is
"<http://www.biodas.org/dtd/dasstyle.dtd>".

<DASSTYLE> (required; one only)  
The appropriate doctype and root tag is DASSTYLE.

<STYLESHEET> (required; one only)  
There is a single <STYLESHEET> tag. Its '''version ''' (required)
attribute indicates the current version of the stylesheet, and can be
used for caching purposes.

<CATEGORY> (required; one or more)  
There are one or more <CATEGORY> tags, each providing information on the
display of a high-level feature category. The **id** (required) tag
uniquely names the category. A special name is "default", which tells
the annotation viewer what format to use for categories that are not
otherwise specified in the stylesheet. Another special name is "group".
A "group" entry indicates the format to use for a particular group
of features.

; <TYPE> (required; one or more per CATEGORY)  
  
There are one or more <TYPE> tags per <CATEGORY>, each providing display
suggestions for one type of annotation. The **id** (required) uniquely
identifies the type. A special id is "default", which, if present,
identifies a default style for the enclosing category.

;; <GLYPH> (required; one or more per TYPE)  
  
There is one or more <GLYPH> tag per <TYPE>. It provides information on
what glyph (graphical widget) to use to display the indicated
annotation type. The optional **zoom** attribute, implements a simple
form of semantic zooming, and allows the client to select the glyph and
its attributes based on the zoom level. Possible values are "high",
"medium" and "low". If multiple <GLYPH> tags are present, this attribute
**must** be present in order to select among them. A "high" zoom means
that there are fewer base pairs per pixel (high magnification). A "low"
zoom shows more base pairs. "Medium" is intermediate. It is left to the
client to determine the boundaries for "high", "medium" and "low", since
this is a function of the graphics rendering.

;;; &lt;*ID*&gt; (required; one per GLYPH)  
  
The ID value refers to a recognized glyph from the glyph types list
(\[\#glyphid see below\]).

;;; &lt;*ATTR*&gt; (optional; one or more per ID)  
  
The recognized ATTR (attributes) are determined by which glyph ID
is specified. See the \[\#glyphid glyph types\] list below for
more information.

Here is a short stylesheet example:

`           ...`  
`     `<CATEGORY id="Similarity">  
`   `<TYPE id="default">  
`        `<GLYPH>  
  
`                  `<LINE>  
`              `<FGCOLOR>`gray`</FGCOLOR>  
`                  `</LINE>  
`        `</GLYPH>  
`   `</TYPE>  
  
`   `<TYPE id="NN">  
`        `<GLYPH >  
`                  `<BOX>  
`              `<HEIGHT>`4`</HEIGHT>  
`              `<FGCOLOR>`black`</FGCOLOR>` `  
`              `<BGCOLOR>`red`</BGCOLOR>  
  
`                  `</BOX>  
`        `</GLYPH>  
`   `</TYPE>  
`   `<TYPE id="NP">  
`        `<GLYPH>  
`                  `<TOOMANY>  
  
`              `<HEIGHT>`4`</HEIGHT>  
`              `<FGCOLOR>`black`</FGCOLOR>  
`              `<BGCOLOR>`blue`</BGCOLOR>  
`                  `</TOOMANY>  
  
`        `</GLYPH>  
`   `</TYPE>  
`   `<TYPE id="PN">  
`        `<GLYPH>  
`                  `<BOX>  
`              `<HEIGHT>`3`</HEIGHT>  
  
`              `<FGCOLOR>`blue`</FGCOLOR>  
`              `<BGCOLOR>`green`</BGCOLOR>  
`                  `</BOX>  
`        `</GLYPH>  
`   `</TYPE>  
  
`   `<TYPE id="PP">  
`        `<GLYPH>  
`                  `<SPAN>  
`              `<HEIGHT>`4`</HEIGHT>  
`              `<FGCOLOR>`gray`</FGCOLOR>  
  
`                  `</SPAN>  
`        `</GLYPH>  
`   `</TYPE>  
`     `</CATEGORY>  
`      ...`  

Groups can also have stylesheet entries. If present, they are located in
the category named "group". Typically a group will be associated with
the "line" glyph, which as described below, draws connections between
the members of a group.

A sample stylesheet used for the WormBase DAS server can be found at
\[sample\_stylesheet.xml
<http://www.biodas.org/documents/sample_stylesheet.xml>\].

### Glyphs and Groups

Glyphs and their attributes are typically applied to individual
features. However, they can be applied to entire groups as well (via the
<GROUP> **type** attribute). In this case, the glyph will apply to the
connecting regions **between** the components of the group.

For example, to indicate that the exons in a "transcript" group should
be drawn with a yellow box, that the utrs should be drawn with a blue
box, and that the connections between exons should be drawn with a
hat-shaped line:

<CATEGORY id="Transcription">  
`   `<TYPE id="exon">  
`      `<GLYPH>  
`         `<BOX>  
`            `<BGCOLOR>`yellow`</BGCOLOR>  
  
`         `</BOX>  
`      `</GLYPH>  
`   `</TYPE>  
  
`   `<TYPE id="utr">  
`      `<GLYPH>  
`         `<BOX>  
  
`            `<BGCOLOR>`blue`</BGCOLOR>  
`         `</BOX>  
`      `</GLYPH>  
`   `</TYPE>  
</CATEGORY>  
  
<CATEGORY id="group">  
<TYPE id="transcript">  
`   `<GLYPH>  
`      `<LINE>  
`         `<FGCOLOR>`black`</FGCOLOR>  
`         `<LINE_STYLE>`hat`</LINE_STYLE>  
  
`      `</LINE>  
`   `</GLYPH>  
</TYPE>  
`...`

------------------------------------------------------------------------

Fetching Sequence Assemblies
----------------------------

Reference servers, but not annotation servers, must represent and serve
genome assemblies.

The components of an assembly are treated as a set of features with a
type *category* attribute of "component" and a *reference* attribute of
"yes". Intermediate components of the assembly will also have a
*subparts* attribute of "yes". Components that are the parents of the
reference sequence in the assembly have a category attribute of
"supercomponent."

### Moving Down in an Assembly

For those components that have subparts, the start and end of the
feature give the feature's position in the requested segment's
coordinate system, and the id, start and end of the <TARGET> element
gives the feature's position in its native coordinates.

For example:

`         1      200         400                1000`  
`         +--------+-----------+-------------------+ `**`chr22`**  
  
`         1      200 220     1 20                620`  
`         +--------+---- A   --+-------------------+ `**`B`**  
  
`            1    80         280     400`  
`            ------+-----------+-------- `**`C`**  
  
`            =================== `**`C.1`**  
`                          ============= `**`C.2`**

A request for this assembly will look like the following:

<code>

[`http://www.wormbase.org/db/das/elegans/features?segment=chr22:1,1000;category=component`](http://www.wormbase.org/db/das/elegans/features?segment=chr22:1,1000;category=component)

</code>

The reference server will return the following (abbreviated) document:

<SEGMENT id="chr22" start="1" stop="1000">  
  
`  `<FEATURE id="chr22">  
`    `<START>`1`</START>  
`    `<STOP>`1000`</STOP>  
`    `<TYPE id="Contig" category="component" reference="yes" superparts="no" subparts="yes">`chr 22`</TYPE>  
  
`    `<TARGET id="chr22" start="1" stop="1000">`chr22`</TARGET>  
`    ...`  
`  `</FEATURE>  
  
<FEATURE id="Contig:A">  
`    `<START>`1`</START>  
  
`    `<STOP>`200`</STOP>  
`    `<TYPE id="Contig" category="component" reference="yes" superparts="yes" subparts="no">`a contig`</TYPE>  
`    `<TARGET id="A" start="1" stop="200">`Contig A`</TARGET>  
`    ...`  
`  `</FEATURE>  
  
`  `<FEATURE id="Contig:B">  
`    `<START>`400`</START>  
`    `<STOP>`1000`</STOP>  
`    `<TYPE id="Contig" category="component" reference="yes" superparts="yes" subparts="no">`a contig`</TYPE>  
  
`    `<TARGET id="B" start="20" stop="620">`Contig B`</TARGET>  
`    ...`  
`  `</FEATURE>  
  
  
`  `<FEATURE id="Contig:C">  
`    `<START>`200`</START>  
  
`    `<STOP>`400`</STOP>  
`    `<TYPE id="Contig" category="component" reference="yes" superparts="yes" subparts="yes">`a contig`</TYPE>  
`    `<TARGET id="C" start="80" stop="280">`Contig C`</TARGET>`    ...`  
`  `</FEATURE>  
  
</SEGMENT>

Notice that contig C is marked as having subparts. This is an indication
to the client that it should emit a features request that includes
segment C:80,280 in order to discover its components (C.1 and C.2).

Notice also that chr22 appears as a component of itself with the
attribute **superparts="no"** and **subparts="yes"**. This is a side
effect of providing information about the component parent.

### Moving Up in an Assembly

It is also desirable for a client to fetch the **parent** of a segment,
so as to accomodate the situation in which the user enters the browser
at a contig or sequenced clone, and wants to "zoom out."

This situation is complicated by rough draft issues, in which a single
rough draft sequence segment may have multiple parents, and some
sections of the segment may not belong in the assembly at all. For
example:

`                        A   B     C   D`  
`           `**`contig21`**`----------->  <-----------`**`contig100`**  
  
`                        |   |    /   /`  
`                        |   |   /   /`  
`             `**`Acc` `A`**` ---------------------`  
`                        a   b  c   d`

Here, the segment "Acc A" contains two fragments, one of which is
located on contig21 and the other on contig100.

To retrieve this information, the client requests the category
**supercomponent**. For segments that are in the middle of the assembly,
one or more assembly parents will be returned in **addition** to
subcomponents. The parent <START>, <STOP> and <ORIENTATION> tags are
presented in the coordinate system of the requested segment, as always.
The **start** and **stop** attributes of the <TARGET> tag, denote the
corresponding segment in the coordinate system of the parent. As always,
start is less than stop, for both the feature and the target.

<SEGMENT id="Acc A" start="1" stop="1000">  
`   `<FEATURE id="contig21_goldenpath_map">  
`      `<START>`a`</START>  
`      `<STOP>`b`</STOP>  
  
`      `<ORIENTATION>`+`</ORIENTATION>  
`      `<TYPE id="Contig" category="supercomponent" reference="yes" superparts="yes" subparts="yes">`a contig`</TYPE>  
`      `<TARGET id="contig21" start="A" stop="B"></TARGET>  
`   `</FEATURE>  
  
`   `<FEATURE id="contig100_goldenpath_map">  
`      `<START>`c`</START>  
`      `<STOP>`d`</STOP>  
`      `<ORIENTATION>`-`</ORIENTATION>  
  
`      `<TYPE id="Contig" category="supercomponent" reference="yes" superparts="yes" subparts="yes">`a contig`</TYPE>  
`      `<TARGET id="contig100" start="D" stop="C"></TARGET>  
`   `</FEATURE>  
</SEGMENT>

To continue following the parents upward in the assembly, the client
will issue further features requests for the target IDs, in this case
"contig21" and "contig100". In the general case, following parents will
project the requested segment onto a discontinuous set of regions,
potentially on different chromosomes. The client may wish to alert the
user and refuse to proceed further when it encounters a segment with
multiple parents.

------------------------------------------------------------------------

Feature Types and Categories
----------------------------

This is a list of generic feature categories and specific feature types
within them. This list was derived from the features currently exported
by ACeDB/GFF and is not comprehensive. Suggestions for modifications,
additions and deletions are welcomed.

### <em>component</em>

This category indicates that the feature is a child component of the
reference sequence in the current assembly. When combined with the
**reference="yes"** attribute, this indicates that the feature can be
used as a reference point to retrieve subfeatures contained within it
(including subcomponents).

### <em>supercomponent</em>

This category indicates that the feature is the parent of the reference
sequence in the current assembly. When combined with the
**reference="yes"** attribute, this indicates that the feature can be
used as a reference point to retrieve features that completely contain
the selected range of the reference sequence.

### <em>translation</em>

The <em>translation</em> category is used for features that relate to
regions of the sequence that are translated into proteins. Features that
relate to transcription are separate (see below).

Features:

-   stop - position of the translation stop codon
-   ATG - position of the start codon
-   CDS - position of the coding region
-   5'UTR - untranslated region
-   3'UTR - untranslated region
-   misc\_translated - miscellaneous

It is recommended, but not required, that the <FEATURE> section contain
<LINK> and/or

<NOTE>
tags that provide further information on the transcription feature.

### <em>transcription</em>

The <em>transcription</em> category is used for features that relate to
regions of the sequence that are transcribed into RNA.

Features:

-   exon
-   intron
-   tRNA
-   mRNA
-   ncRNA
-   5'Cap - transcriptional start site
-   PolyA
-   Splice5 - splice donor
-   Splice3 - splice acceptor
-   misc\_transcribed

It is recommended, but not required, that the <FEATURE> section contain
<LINK> and/or

<NOTE>
tags that provide further information on the transcription feature.

### <em>variation</em>

The <em>variation</em> category is used for features that relate to
regions of the sequence that are polymorphic.

Features:

-   insertion
-   deletion
-   substitution
-   misc\_variation

It is recommended, but not required, that the <FEATURE> section contain
<LINK> and/or

<NOTE>
tags that provide further information on the variation.

### <em>structural</em>

The <em>structural</em> category is used for features that relate to
mapping, sequencing and assembly, as well as for various landmarks that
carry no intrinsic biological information.

Features:

-   clone
-   primer\_left
-   primer\_right
-   oligo
-   assembly\_tag
-   misc\_structural

It is recommended, but not required, that the <FEATURE> section contain
<LINK> and/or

<NOTE>
tags that provide further information on the structural feature.

### <em>similarity</em>

The <em>similarity</em> category is used for areas that are similar to
other sequences. Similarity features should have a <METHOD> tag that
indicates the algorithm used for the sequence comparison, and a <TARGET>
tag that indicates the target of the match.

Features:

-   NN -- nucleotide to nucleotide similarity (e.g. blastn)
-   NP -- nucleotide to protein similarity (e.g. blastx)
-   PN -- protein to nucleotide similarity (e.g. tblastn)
-   PP -- protein to protein similarity (e.g. tblastx)
-   misc\_homology

### <em>repeat</em>

The <em>repeat</em> category is used for areas that contain repetitive
DNA. This category is used both for low-complexity regions, such as
microsatellites, and for more biologically interesting features, such as
transposon insertion sites.

Features:

-   microsatellite
-   inverted
-   tandem
-   transposable\_element
-   LINE - long repeat not definitely identified as a transposon
-   misc\_repeat

It is recommended, but not required, that the <FEATURE> section contain
<LINK> and/or

<NOTE>
tags that provide further information on the repetitive element.

### <em>experimental</em>

The <em>experimental</em> category is a catchall used to flag areas
where there is interesting experimental data of one sort or another. It
is intended for use with high-throughput functional genomics work, such
as knockouts or insertional mutagenesis screens.

Features:

-   knockout
-   expression\_tag
-   microarrayed
-   RNAi\_result
-   transgenic
-   mutant - a mutant phenotype associated with region
-   misc\_experimental

It is recommended, but not required, that the <FEATURE> section contain
<LINK> and/or

<NOTE>
tags that provide further information on the nature of the experimental
data.

------------------------------------------------------------------------

Glyph Types
-----------

This section describes a set of generic "glyphs" that can be used by
sequence display programs to display the position of features on a
sequence map. The annotation server may use these glyphs to send display
suggestions to the viewer via the \[\#stylesheet stylesheet document\].

The current set of glyph ID values are:

-   ARROW
-   ANCHORED\_ARROW
-   BOX
-   CROSS
-   EX
-   HIDDEN
-   LINE
-   SPAN
-   TEXT
-   TOOMANY
-   TRIANGLE
-   PRIMERS

Each glyph has a set of attributes associated with it. Attribute values
come in the following flavors:

INT  
An integer

FLOAT  
A floating point number (not currently used)

STRING  
A text string

COLOR  
A color. Colors can be specified using the "\#RRGGBB" format commonly
used in HTML, or as one of the 16 IBM VGA colors recognized by Netscape
and Internet Explorer.

BOOL  
A boolean value, either "yes" or "no".

FONT  
A font. Any of the font identifiers recognized by Web browsers is
acceptable, e.g. "helvetica".

FONT\_STYLE  
One of "bold", "italic", "underline".

LINE\_STYLE  
One of "hat", "solid", "dashed".

Some attributes are shared by all glyphs. Others are glyph-specific. The
following attributes are shared in common:

HEIGHT  
type: INT

The height of the glyph, in pixels. For the text font, this is
equivalent to the FONTSIZE attribute.

FGCOLOR  
type: COLOR

The foreground color of the glyph. This is the line and outline color
for graphical glyphs, and the font color for text glyphs.

BGCOLOR  
type: COLOR

The background color of the glyph. For hollow glyphs, such as boxes,
this is the color of the interior of the box. For solid glyphs, such as
text, this is ignored

LABEL  
BOOL

Whether the glyph should be labeled with its name, as dictated by the
<FEATURE> **label** attribute in the DASGFF document.

BUMP  
BOOL

Whether the glyph should "bump" intersecting glyphs so that they do
not overlap.

### <em>ARROW</em>

A double-headed arrow with an axis either orthogonal or parallel to the
sequence map.

Attributes:

PARALLEL  
type: BOOL

Arrows run either parallel ("yes") or orthogonal("no") to the
sequence axis.

### <em>ANCHORED\_ARROW</em>

An arrow that has an arrowhead at one end, and an "anchor" (typically a
diamond or line) at the other. The arrow points in the direction
indicated by the <ORIENTATION> tag.

Attributes:

PARALLEL  
type: BOOL

Arrows run either parallel ("yes") or orthogonal("no") to the
sequence axis.

### <em>BOX</em>

A rectangular box.

Attributes:

LINEWIDTH  
type: INT

Width of the box outline.

### <em>CROSS</em>

A cross "+". Common used for point mutations and other point-like
features.

Attributes:

(no glyph-specific attributes)

### <em>DOT</em>

A dot. Common used for point mutations and other point-like features.

Attributes:

(no glyph-specific attributes)

### <em>EX</em>

"X" marks the spot. Common used for point mutations and other point-like
features.

Attributes:

(no glyph-specific attributes)

### <em>HIDDEN</em>

A feature that is invisible, intended to support semantic zooming
schemes in which a feature is hidden at particular zooms.

Attributes: none.

### <em>LINE</em>

A line. Lines are equivalent to arrows with both the <em>northeast</em>
and <em>southwest</em> attributes set to "no".

Attributes:

STYLE  
type: LINE\_STYLE

The line type. A type of "hat" draws an inverted V (commonly used
for introns). A type of "solid" draws a horizontal solid line in the
indicated color. A type of "dashed" draws a dashed horizonal line in the
indicated color.

### <em>SPAN</em>

A spanning region, the recommended representation is a horizontal line
with vertical lines at each end.

Attributes:

(no glyph-specific attributes)

### <em>TEXT</em>

A bit of text.

Attributes:

FONT  
type: FONT

The font.

FONTSIZE  
type: INT

The font size.

STRING  
type: STRING

The text to render.

STYLE  
type: FONT\_SYTLE

The style in which to render this glyph. Multiple FONT\_STYLE attributes
may be present.

### <em>PRIMERS</em>

Two inward-pointing arrows connected by a line of a different color.
Used for showing primer pairs and a PCR product. The length of the
arrows is meaningless.

There are no glyph-specific attributes, but in this context the
foreground color is the color of the arrows, and the background color is
the color of the line that connects them.

### <em>TOOMANY</em>

Too many features than can be shown. Recommended for use in
consolidating sequence homology hits. The recommended visual
presentation is a set of overlapping boxes.

Attributes:

LINEWIDTH  
type: INT

Width of the glyph.

### <em>TRIANGLE</em>

A triangle. Commonly used for point mutations and other point-like
features. The triangle is always drawn in the center of its range, but
its width and height can be controlled by HEIGHT and LINEWIDTH
respectively.

Attributes:

LINEWIDTH  
type: INT

Width of the glyph.

DIRECTION  
One of "N", "E", "S", and "W"

------------------------------------------------------------------------

Other Issues
------------

The distributed annotation system must have a mechanism for detecting
and resolving version skew across reference and annotation servers.
Although one such mechanism is currently incorporated into the
ACeDB-based prototype, it is largely untested and hence not yet a part
of the DAS standard.

Changes
-------

This section was added at version 0.99.

### Version 1.51

1.  The description of the entry\_points document was out of synch with
    the DTD. Also there seems to have been some semantic drift between
    Dazzle, the UCSC server, and LDAS with regards to the attributes of
    the <SEGMENT> tag. This has now been made explicit, and the DTD
    relaxed to allow all styles.

### Version 1.5

1.  Added capabilities header.
2.  Added exception handling for invalid sequence IDs.
3.  Added feature\_id request.
4.  Corrected syntax errors in stylesheet example.

### Version 1.01

1.  Split assembly functionality into "component" and "supercomponent".
2.  Removed redundant descriptions of glyph attributes.

### Version 1.0

1.  Removed deprecated *resolve* command.
2.  Removed deprecated *entry\_points* **ref** argument.
3.  Added **superparts** attribute to DASGFF <FEATURE> tag.
4.  New discussion of how to move upwards in an assembly.
5.  Reorganized specification to put responses close to requests.
6.  Added a stylesheet example document.
7.  Normalized the names of glyph COLOR and FILLCOLOR attributes to
    FGCOLOR and BGCOLOR.
8.  Added the LABEL attribute to all glyphs.
9.  Added the STYLE attribute to the LINE glyph.
10. Added the ability to assign a glyph to a group.
11. Added HIDDEN glyph.

### Version 0.999

1.  Added LINK, NOTE, and TARGET to FEATURE
2.  Added section entitled "Fetching Sequence Assemblies"

### Version 0.998

1.  Deprecated regular expression matching for types and categories.
2.  Allow multiple TYPE arguments for logical OR filtering.
3.  Made FEATURE optional within features return document.
4.  Made TYPE optional within types return document.

### Version 0.996

1.  Added subparts tag to features and entry\_points.
2.  Removed the requirement that the server return features that do not
    overlap with the requested segment.
3.  Added support for multiple segments/sequences in types document.

### Version 0.995

1.  Added support for multiple segments/sequences in returned documents.
2.  Added support for assembly components.

### Version 0.99

1.  Allow query parameters to be POSTed to the DAS URL.
2.  Added compatibility warning about SOAP conversion.
3.  Use Version 8 regular expressions rather than GNU's, giving
    compatibility with both Perl regex and GNU regex.
4.  Made the **id** attribute of the <TYPE> tag required.
5.  Changed the WIDTH glyph attribute to HEIGHT throughout.

------------------------------------------------------------------------

Lincoln D. Stein, lstein@cshl.org  
[Cold Spring Harbor Laboratory](http://www.cshl.org/)

Last modified: Thu May 22 12:55:39 EDT 2003