---
title: SpecReview
permalink: wiki/SpecReview/
layout: wiki
---

<h1>
Distributed Sequence Annotation System (DAS) Spec Review

</h1>
<h3 align=CENTER>
Version 2.53

</h3>
<h5 align=CENTER>
Oct 1,2008

</h5>
This is a proposal for a reworked DAS specification which hopes to
clarify the DAS spec based on how DAS is being used in the community
today and to include commands from the
<a href="http://www.dasregistry.org/spec_1.53E.jsp">1.53E spec</a> and
some of the 2.0 spec.

`Also the document has been adjusted to reflect changes in the use of DAS away from a solely genome centric protocol to a more open one encompassing other reference/coordinate systems such as alignments and protein. The spec also includes references to the DAS Registry which is essential for `  
`implementing an SOA architecture. Note: this is a technical document but should be readable and understandable by people without a deep understanding of broader technical issues and other system architectures.`

Proposed modifications to the spec are indicated in brown.

<h2>
<a name="description">Overview of the System</a>

</h2>
This section provides a high-level view of the system architecture.

<h3>
<a name="components">System Components (Co-ordinate System Servers,
Annotation Servers and the DAS Registry)</a>

</h3>
The DAS system consists of the a reference server, one or more
annotation servers and the das registry.

<h4>
<a name="referenceServer">Reference Server</a>

</h4>
Central to the DAS system is the idea of reference objects that can be
served from a reference server. These are biological data objects with
stable identifiers which are targets for annotation.

`In the original DAS protocol, the reference objects are always biological sequences: chromosomes, scaffold sequences from genome assemblies or protein sequences.`  
` When a DAS client starts, its first action is to connect to an appropriate reference server and retrieve the reference sequence.`  
`  Once a reference object has been loaded, the DAS client will contact one or more annotation servers to obtain the data provided for the reference objects.`  
`   Typical annotations might include sets of predicted exons on a genome sequence, or matches to a protein domain model on a protein sequence.`  
`    It is the client's responsibility to collect all relevant annotations and display them in a user-friendly manner.`

A coordinate system describes the data that is made available by a DAS
source/reference server. This information is important for the DAS
clients, to deal with data correctly, as they often can accept data
served in multiple coordinate systems. The coordinate systems are
described by four fields: Authority, (assembly) Version, Type, and
Organism. The assembly version is important for genome assemblies, but
not really applicable for other datasets like UniProt sequences,
therefore this field is optional.

<h4>
<a name="annotationServer">Annotation Server</a>

</h4>
Annotation servers are specialized for returning lists of annotations
across a reference object served by the reference server. Each
annotation can be anchored to the co-ordinate system map by way of a
start and stop position relative to one of the reference objects.

` Annotations have an ID that is unique to`

the server and a structured description that describes its nature and
attributes. Annotations may also be associated with Web URLs that
provide additional human readable information about the annotation.

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

<h4>
<a name="dasRegistry">The DAS Registry</a>

</h4>
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

<h2>
<a name="client_server">Client/Server Interactions</a>

</h2>
The DAS is Web-based. Clients query the reference and annotation servers
by sending a formatted URL request to the server. This request must
follow the conventions of the HTTP/1.0 protocol (see <a
href="ftp://ftp.isi.edu/in-notes/rfc2616.txt">RFC2616</a>. Servers
process the request and return a response in the form of a formatted XML
document (see <a href="http://www.w3.org/XML/">W3C Extensible Markup
Language</a>).

<h3>
<a name="request">The Request</a>

</h3>
All DAS requests take the form of a URL. Each URL has a site-specific
prefix, followed by a standardized path and query string. The
standardized path begins with the string <b>/das</b>. This is followed
by URL components containing the data source name and a command. For
example:

>     http://www.wormbase.org/db/das/elegans/features?segment=CHROMOSOME_I:1000,2000
>     ^^^^^^^^^^^^^^^^^^^^^^^^^^ ^^^ ^^^^^^^ ^^^^^^^^ ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
>        site-specific prefix    das  data   command  arguments
>                                      src 

<font color="red">give some more examples here</font> In this case, the
site-specific prefix is <b><http://www.wormbase.org/db></b>. The request
begins with the standardized path <b>/das</b>, and the data source, in
this case <b>/elegans</b>. This is followed by the command
<b>/features</b>, which requests a list of features, and a query string
providing named arguments to the <b>/features</b> command.

The data source component allows a single server to provide information
on several genomes.

More information on the format of the request and the various available
commands is given <a href="#commands">below</a>.

The query string portion of the request (the "?" symbol rightward) can
be POSTed to the URL following conventional HTTP standards. Since some
queries can be quite large, this is the recommended way of argument
passing.

<h3>
<a name="response">The Response</a>

</h3>
The response from the server to the client consists of a standard HTTP
header with DAS status information within that header followed
optionally by an XML file that contains the answer to the query. The DAS
status portion of the header consists of two lines. The first is
X-DAS-Version and gives the current protocol version number, currently
DAS/1.0. The second line is X-DAS-Status and contains a three digit
status code which indicates the outcome of the request.

Here is an example HTTP header: (<i>provided by Web server</i>)

>     HTTP/1.1 200 OK                          
>     Date: Sun, 12 Mar 2000 16:13:51 GMT          
>     Server: Apache/1.3.6 (Unix) mod_perl/1.19    
>     Last-Modified: Fri, 18 Feb 2000 20:57:52 GMT 
>     Connection: close                            
>     Content-Type: text/plain                     
>     X-DAS-Version: DAS/1.5
>     X-DAS-Status: 200
>     X-DAS-Capabilities: error-segment/1.0; unknown-segment/1.0; unknown-feature/1.0; ...
>     <cite>data follows...</cite>

The defined status codes are listed in Table 1.

<table border>
<caption>
Table 1: DAS response codes

</caption>
<tr>
<th>
200

</th>
<td>
OK, data follows

</td>
</tr>
<tr>
<th>
400

</th>
<td>
Bad command (command not recognized)

</td>
</tr>
<tr>
<th>
401

</th>
<td>
Bad data source (data source unknown)

</td>
</tr>
<tr>
<th>
402

</th>
<td>
Bad command arguments (arguments invalid)

</td>
</tr>
<tr>
<th>
403

</th>
<td>
Bad reference object (reference sequence unknown)

</td>
</tr>
<tr>
<th>
404

</th>
<td>
Bad stylesheet (requested stylesheet unknown)

</td>
</tr>
<tr>
<th>
405

</th>
<td>
Coordinate error (sequence coordinate is out

`     of bounds/invalid)`

</td>
</tr>
<tr>
<th>
500

</th>
<td>
Server error, not otherwise specified

</td>
</tr>
<tr>
<th>
501

</th>
<td>
Unimplemented feature

</td>
</tr>
</table>
The HTTP/1.0 protocol allows web clients to request byte-level
compression of the response by sending the HTTP header
<i>Accept-Encoding</i> header. Web servers that are capable of it can
reply with a <i>Content-transfer-encoding</i> header and a compressed
body. Implementors of DAS clients and servers may wish to implement this
HTTP feature.

<table bgcolor="#DEB887" border="1">
<tr>
<th>
New in version 1.5

</th>
</tr>
<tr>
<td>
`     The X-Das-Capabilities header provides an extensible list of the`  
`     capabilities that the server provides.  This can be used by those`  
`     writing experimental extensions to DAS to flag clients that those`  
`     extensions are available.  Capabilities have the form`  
`     `<i>`CapabilityName/Version`</i>` and are separated by semicolon,`  
`     space, as in "capabilityA/1.0; capabilityB/1.4;`  
`     capabilityC/1.0". The  following standard capabilities`  
`     are present in the DAS/1.5 protocol:`  
`     `

<table border="1">
<tr>
<th>
Capability Name

</th>
<td>
Description

</td>
</tr>
<tr>
<th align="LEFT">
dsn/1.0

</th>
<td>
The server supports the basic <i>dsn</i> request.

</tr>
<tr>
<th align="LEFT">
dna/1.0

</th>
<td>
The server supports the basic <i>dna</i> request.

</tr>
<tr>
<th align="LEFT">
types/1.0

</th>
<td>
The server supports the basic <i>types</i> request.

</tr>
<tr>
<th align="LEFT">
stylesheet/1.0

</th>
<td>
The server supports the basic <i>stylesheet</i> request.

</tr>
<tr>
<th align="LEFT">
features/1.0

</th>
<td>
The server supports the basic <i>features</i> request.

</tr>
<tr>
<th align="LEFT">
entry\_points/1.0

</th>
<td>
The server supports the basic <i>entry\_points</i> request.

</tr>
<tr>
<th align="LEFT">
error-segment/1.0

</th>
<td>
Server will report requests for invalid segments with an

`     <ErrorSegment> response.`  
`   `

</tr>
<tr>
<th align="LEFT">
unknown-segment/1.0

</th>
<td>
Server will report requests for unknown or unannotated segments with an

`     <UnknownSegment> response.`  
`   `

</tr>
<tr>
<th align="LEFT">
unknown-feature/1.0

</th>
<td>
Server will report requests for unknown features with an

`     <UnknownFeature> response.`  
`   `

</tr>
<tr>
<th align="LEFT">
feature-by-id/1.0

</th>
<td>
The <i>features</i> request will accept the CGI parameter "feature\_id",
enabling

`         the server to look up segment(s) based on the ID of a feature.`  
`   `

</tr>
<tr>
<th align="LEFT">
group-by-id/1.0

</th>
<td>
The <i>features</i> request will accept the CGI parameter "group\_id",
enabling

`         the server to look up segment(s) based on the ID of a group.`  
`   `

</tr>
<tr>
<th align="LEFT">
component/1.0

</th>
<td>
The <i>features</i> request will return components of the indicated
segment when

`         a category type of "component" is requested.`  
`   `

</tr>
<tr>
<th align="LEFT">
supercomponent/1.0

</th>
<td>
The <i>features</i> request will return supercomponents of the indicated
segment when

`         a category type of "supercomponent" is requested.`  
`   `

</tr>
<tr>
<th align="LEFT">
sequence/1.0

</th>
<td>
The server supports the new <i>sequence</i> request.

</tr>
</table>
</td>
</tr>
</table>
<hr>
<h3>
<a name="co-ordinateSystem">The Co-ordinate System</a>

</h3>
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

To give a concrete example, the <cite>C. elegans</cite> reference map
consists of six chromosome-length entry points. Each chromosome is
formed from several contigs called "superlinks", and each superlink
contains one or more smaller contigs called "links". Links in turn are
composed of one or more fully-sequenced clones. One could refer to an
annotation by specifying its start or stop positions in clone, link,
superlink, or chromosome coordinates. The distributed annotation system
automatically converts any coordinate system into any other. Because
coordinates within clones are more stable to revisions than coordinates
within links or chromosomes, it is recommended that annotation
coordinates be stored relative to the smallest sequencing unit. The
hierarchy is extensible. If the <cite>C. elegans</cite> gene predictions
were stable, it would make sense to store certain annotations, such as
the positions of exons, relative to the transcriptional unit.

<h3>
<a name="ids">Reference sequence IDs</a>

</h3>
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

       Client request       server's internal id         Response to client

       gi-123456       -->  gi:123456                    --->  gi-123456

       gi-123456:1,1000 --> gi:123456 start=1 stop=1000  --->  gi-123456:1,1000

<hr>
<h2>
<a name="queries">The Queries</a>

</h2>
<h3>
<a name="SourcesCmd">Sources</a>

</h3>
<font color="red">DSN command has been deprecated in favour of the
sources cmd</font>

In particular the following information for a DAS server is important:

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

</div>
This section lists the queries recognized by reference and annotation
servers. Each of these queries begins with some site-specific prefix,
denoted here as <i>PREFIX</i>. The other meta-variable used in these
examples is <i>DSN</i>, which is a symbolic data source. (As seen the
the <a href="#request" target="request"> above example.</a>) Data
sources are standardized across DAS servers in such a way that a data
source name has a one-to-one correspondence with a reference sequence.

<h3>
<a name="entry">Retrieve the List of Entry Points for a Data Source
<font color="red">This Entry\_Points cmd is now mandatory for reference
servers.</font></a>

</h3>
<b>Scope:</b> Reference servers.

<b>Command:</b> <i>entry\_points</i>

<b>Format:</b>

>     <i>PREFIX</i>/das/<i>DSN</i>/entry_points

<b>Description:</b> This query returns the list of sequence entry points
available and their sizes in base pairs.

<b>Arguments:</b>

<h4>
Response:

</h4>
The response to the <i>entry\_points</i> command is the "DASEP"
XML-formatted document:

<b>Format:</b>

>     &lt;?xml version="1.0" standalone="no"?&gt;
>     &lt;!DOCTYPE DASEP SYSTEM "http://www.biodas.org/dtd/dasep.dtd"&gt;
>     &lt;DASEP&gt;
>       &lt;ENTRY_POINTS href="<i>url</i>" version="<i>X.XX</i>"&gt;
>         &lt;SEGMENT id="<i>id1</i>" start="<i>start1</i>" stop="<i>stop1</i>" type="type" orientation="+"&gt;descriptive text&lt;/SEGMENT&gt;
>
>         &lt;SEGMENT id="<i>id2</i>" start="<i>start2</i>" stop="<i>stop2</i>" type="type" orientation="+"&gt;descriptive text&lt;/SEGMENT&gt;
>         &lt;SEGMENT id="<i>id3</i>" start="<i>start3</i>" stop="<i>stop3</i>" type="type" orientation="+"&gt;descriptive text&lt;/SEGMENT&gt;
>
>         ...
>       &lt;/ENTRY_POINTS&gt;
>     &lt;/DASEP&gt;

<dl>
<dt>
&lt;!DOCTYPE&gt; (required; one only)

<dd>
The doctype indicates which formal DTD specification to use.

`     For the entry_points query, the doctype DTD is "`[`http://www.biodas.org/dtd/dasep.dtd`](http://www.biodas.org/dtd/dasep.dtd)`".`  
`     `

<dt>
&lt;DASEP&gt; (required, one only)

<dd>
The appropriate doctype and root tag is DASEP.

<dt>
&lt;ENTRY\_POINTS&gt; (required, only one)

<dd>
There is a single &lt;ENTRY\_POINTS&gt; tag. It has a version

`     number (required) in the form "N.NN".  Whenever the `  
`     DNA of the entry point changes, the version number should change as well.`  
`     `

`     The `<b>`href`</b>` (required) attribute echoes the URL query that`  
`     was used to fetch the current document.`  
`     `

<dt>
&lt;SEGMENT&gt; (optional; zero or more)

<dd>
Each segment contains the attributes <b>id</b>, <b>start</b>,
<b>stop</b>

`     and `<b>`orientation`</b>`.  The id is a unique identifier, which can be used as `  
`     the reference ID in further requests to DAS.  The start and stop indicate the`  
`     start and stop positions of the segment. `<font color="red">`The start and stop are now optional`</font>`.  Orientation is one of "+" or "-" and`  
`     indicates the strandedness of the segment (use "+" if the segment is not intrinsically`  
`     ordered).`<font color="red">`The orientation is now optional.`</font>`.`  
`     `

`     `<font color="red">`The subparts section is now deprecated?`</font>`If the optional `<b>`subparts`</b>` attribute is present and has the value "yes",`  
`     it indicates that the segment has subparts.`  
`     `

`     `<font color="red">`The type should now be a SO type if possible?`</font>  
`     If the optional `<b>`type`</b>` attribute is present, it can be used to describe the`  
`     type of the segment (for future compatibility with Sequence Ontology-based feature`  
`     typing).`  
`     `

`     For compatibility with older versions of the specification, the <SEGMENT>`  
`     tag can use a `<b>`size`</b>` attribute rather than `<b>`start`</b>` and `<b>`stop`</b>`, and`  
`     can omit the `<b>`orientation`</b>` attribute:`  
`     `


          &lt;SEGMENT id="id" size="123456"&gt;
          

`     In this case, the start is implied to be "1" and the stop is implied to be the same`  
`     as the length.`

</dl>
<hr>
<h3>
<font color="red">Retrieve the DNA Associated with a Subsequence has
been deprecated as the sequence cmd below is more commonly used.</font>

</h3>
<hr>
<h3>
<a name="sequence">Retrieve the Sequence Associated with a
Subsequence</a>

</h3>
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
<hr>
<h3>
<a name="types">Retrieve the Types Available for a Segment</a>

</h3>
<b>Scope:</b> Annotation and reference servers.

<b>Command:</b> <i>types</i>

<b>Format:</b>

>     <i>PREFIX</i>/das/<i>DSN</i>/types [?segment=<i>RANGE</i>]
>                                        [;segment=<i>RANGE</i>]
>                                        [;type=<i>TYPE</i>]
>                                        [;type=<i>TYPE</i>]

<b>Description:</b> This query returns the annotation available for a
segment of sequence.

<b>Arguments:</b>

<dl>
<dt>
<b>segment</b> (optional)

<dd>
This is the sequence range. It uses the format format
<i>reference:start,stop</i>, where

`     `<i>`reference`</i>` is the ID of the reference sequence used to establish the coordinate`  
`     system, and `<i>`start`</i>` and `<i>`stop`</i>` are the endpoints of the region to query, inclusive.`  
`     `

<dt>
<b>type</b> (optional)

<dd>
One or more type IDs to be used for filtering annotations on the type

`     field.  If multiple type names are provided, the resulting list of features`  
`     will be the logical OR of the list.`  
`     `

`     For compatibility with versions 0.997 and earlier of this protocol, servers`  
`     are allowed to treat the type ID as a regular expression, but this feature is`  
`     `<b>`deprecated`</b>` and should not be used.`

</dl>
If one or more segment arguments are provided, the list of types
returned is restricted to the indicated segments. If no segment argument
is provided, then <b>all</b> feature types known to the source are
returned.

<h4>
Response:

</h4>
The document returned from the <i>types</i> request is an XML-formatted
"DASTYPES" documents. This is a shortened form of the full features
format (see below) and is used to summarize the type and number of each
annotation. Annotation types can be grouped into segments, or be totaled
across the entire database.

>     &lt;?xml version="1.0" standalone="no"?&gt;
>     &lt;!DOCTYPE DASTYPES SYSTEM "http://www.biodas.org/dtd/dastypes.dtd"&gt;
>
>     &lt;DASTYPES&gt;
>       &lt;GFF version="1.0" href="url"&gt;
>       &lt;SEGMENT id="<i>id</i>" start="<i>start</i>" stop="<i>stop</i>" type="<i>type</i>" version="X.XX" label="<i>label</i>"&gt;
>
>          &lt;TYPE id="<i>id1</i>" method="<i>method</i>" category="<i>category</i>"&gt;<i>Type Count 1</i>&lt;/TYPE&gt;
>          &lt;TYPE id="<i>id2</i>" method="<i>method</i>" category="<i>category</i>"&gt;<i>Type Count 2</i>&lt;/TYPE&gt;
>
>          ...
>       &lt;/SEGMENT&gt;
>       &lt;/GFF&gt;
>     &lt;/DASTYPES&gt;

<dl>
<dt>
&lt;!DOCTYPE&gt; (required; one only)

<dd>
The doctype indicates which formal DTD specification to use.

`     For the types query, the doctype DTD is "`[`http://www.biodas.org/dtd/dastypes.dtd`](http://www.biodas.org/dtd/dastypes.dtd)`".`  
`     `

<dt>
&lt;DASTYPES&gt; (required; one only)

<dd>
The appropriate doctype and root tag is DASTYPES.

<dt>
&lt;GFF&gt; (required; one only)

<dd>
There is a single &lt;GFF&gt; tag. Its <b>version</b> (required)

`     attribute indicates the current version of the XML form of the`  
`     General Feature Format.  The current version is (arbitrarily) 1.0.`  
`     The `<b>`href`</b>` (required) attribute`  
`     echoes the URL query that was used to fetch the current document.`  
`     `

<dt>
&lt;SEGMENT&gt; (required; one or more)

<dd>
The &lt;SEGMENT&gt; tag provides information

`     on the reference segment.  The `<b>`id`</b>`, `<b>`start`</b>` and `<b>`stop`</b>  
`     attributes indicate the coordinate system of the segment, and are required`  
`     if the segment corresponds to a defined region of the genome, optional if`  
`     the list of types corresponds to the entire database.`  
`     The `<b>`version`</b>` attribute (required) indicates the current version of`  
`     the sequence map.  The optional `<b>`label`</b>` attribute supplies a human readable label`  
`     for display purposes.  The optional `<t>`type`</b>` attribute describes the segment`  
`     type, for future compatibility with Sequence Ontology-based feature typing.`  
`     `

<dt>
&lt;TYPE&gt; (optional; zero or more per SEGMENT)

<dd>
Each segment has zero or more &lt;TYPE&gt; tags, which summarize

`   the types of annotation available.  The attributes are `  
`   `<b>`id`</b>` (required), which is a unique id for the annotation type and `  
`   can be used to retrieve further information from the annotation `  
`   server (see `<a href="#feature_linking">`Linking to a`  
`     Feature`</a>`), `<b>`method`</b>` (optional), which indicates the method (subtype)`  
`     for the feature type and the `<b>`category`</b>` (optional)`  
`     attribute, which provides functional`  
`   grouping to related types.  The tag contents (optional) is `  
`     a count of the number of features of this type across the segment.`

</dl>
<hr>
<h3>
<a name="features">Retrieve the Annotations Across a Segment</a>

</h3>
<b>Scope:</b> Reference and annotation servers.

<b>Command:</b> <i>features</i>

<b>Format:</b>

>     <i>PREFIX</i>/das/<i>DSN</i>/features?segment=<i>REF:start,stop</i>[;segment=<i>REF:start,stop</i>...]
>                                           [;type=<i>TYPE</i>]
>                                           [;type=<i>TYPE</i>]
>                                           [;category=<i>CATEGORY</i>]
>                                           [;category=<i>CATEGORY</i>]
>                                           [;categorize=<i>yes|no</i>]
>                                           <font style="background-color: #DEB887">[;feature_id=ID]</font>
>
>                                           <font style="background-color: #DEB887">[;group_id=ID]</font>

<b>Description:</b> This query returns the annotations across one or
more segments of sequence.

<b>Arguments:</b>

<dl>
<dt>
<b>segment</b> (<font style="background-color: #DEB887">zero</font> or
more)

<dd>
If specified, the segment argument restricts the list of annotations to
those that

`     overlap the indicated range.  Each segment argument uses the format format`  
`     `<i>`reference:start,stop`</i>`, where`  
`     `<i>`reference`</i>` is the ID of the reference sequence used to establish the coordinate`  
`     system, and `<i>`start`</i>` and `<i>`stop`</i>` are the endpoints of`  
`     the region to query, inclusive.  Multiple segments may be specified.`  
`     `

<dt>
<b>type</b> (zero or more)

<dd>
Zero or more type IDs to be used for filtering annotations on the type

`     field.  If multiple type names are provided, the resulting list of features`  
`     will be the logical OR of the list.`  
`     `

`     For compatibility with versions 0.997 and earlier of this protocol, servers`  
`     are allowed to treat the type ID as a regular expression, but this feature is`  
`     `<b>`deprecated`</b>` and should not be relied on.`  
` `

<dt>
<b>category</b> (zero or more)

<dd>
Zero or more category IDs to be used for filtering annotations by
category.

`     If multiple categories are provided, they are treated as the logical OR.`  
`     `

`     For compatibility with versions 0.997 and earlier of this protocol, servers`  
`     are allowed to treat the type ID as a regular expression, but this feature is`  
`     `<b>`deprecated`</b>` and should not be relied on.`  
`     `

<dt>
<b>categorize</b> (optional)

<dd>
Either "yes" or "no" (default). If "yes", then each annotation

`     must include its functional category.`  
` `

<dt>
<b style="background-color: #DEB887">feature\_id</b><font style="background-color: #DEB887">
(zero or more; new in 1.5)</font>

<dd style="background-color: #DEB887">
`     Instead of, or in addition to, `<b>`segment`</b>` arguments, you may`  
`     provide one or more `<b>`feature_id`</b>` arguments, whose values`  
`     are the identifiers of particular features.  If the server`  
`     supports this operation, it will translate the feature ID into`  
`     the segment(s) that strictly enclose them and return the result`  
`     in the `<i>`features`</i>` response.  It is possible for the server`  
`     to return multiple segments if the requested feature is present`  
`     in multiple locations.`  
`     `

</dd>
<dt>
<b style="background-color: #DEB887">group\_id</b><font style="background-color: #DEB887">
(zero or more; new in 1.5)</font>

<dd style="background-color: #DEB887">
`     The `<b>`group_id`</b>` argument, is similar to `<b>`feature_id`</b>`, but`  
`     retrieves segments that contain the indicated feature group.`  
`     `

</dd>
</dl>
Annotation servers are only required to return annotations which are
completely contained within the indicated segment. Servers <b>may</b>
also return annotations which overlap the segment, but are not
completely contained within them. Annotations <b>must</b> be returned
using the coordinate system in which they were requested. For example,
if a contig ID was used to specify the segment, then the annotation
endpoints must use contig coordinates.

If multiple segment arguments are provided and they happen to overlap,
then the annotation server may return the same annotation multiple
times, possibly using different coordinate systems. It is the
responsibility of the client to merge annotations based on the assembly.

<h4>
<a name="return_feats">Response:</a>

</h4>
The document returned from the <i>features</i> request is an
XML-formatted "DASGFF" document.

<b>Format:</b>

>     &lt;?xml version="1.0" standalone="no"?&gt;
>
>     &lt;!DOCTYPE DASGFF SYSTEM "http://www.biodas.org/dtd/dasgff.dtd"&gt;
>     &lt;DASGFF&gt;
>       &lt;GFF version="1.0" href="url"&gt;
>       &lt;SEGMENT id="<i>id</i>" start="<i>start</i>" stop="<i>stop</i>" type="<i>type</i>" version="X.XX" label="<i>label</i>"&gt;
>
>           &lt;FEATURE id="<i>id</i>" label="<i>label</i>"&gt;
>              &lt;TYPE id="<i>id</i>" category="<i>category</i>" reference="<i>yes|no</i>"&gt;<i>type label</i>&lt;/TYPE&gt;
>
>              &lt;METHOD id="<i>id</i>"&gt;<i> method label </i>&lt;/METHOD&gt;
>              &lt;START&gt;<i> start</i> &lt;/START&gt;
>              &lt;END&gt;<i> end</i> &lt;/END&gt;
>
>              &lt;SCORE&gt;<i> [X.XX|-]</i> &lt;/SCORE&gt;
>              &lt;ORIENTATION&gt; [0|-|+] &lt;/ORIENTATION&gt;
>              &lt;PHASE&gt; [0|1|2|-]&lt;/PHASE&gt;
>
>              &lt;NOTE&gt; <i>note text</i> &lt;/NOTE&gt;
>          &lt;LINK href="<i>url</i>"&gt; <i>link text</i> &lt;/LINK&gt;
>          &lt;TARGET id="id" start="x" stop="y"&gt;<i>target name</i>&lt;/TARGET&gt;
>
>          &lt;GROUP id="<i>id</i>" label="<i>label</i>" type="<i>type</i>"&gt;
>                &lt;NOTE&gt; <i>note text</i> &lt;/NOTE&gt;
>                &lt;LINK href="<i>url</i>"&gt; <i>link text</i> &lt;/LINK&gt;
>
>                &lt;TARGET id="id" start="x" stop="y"&gt;<i>target name</i>&lt;/TARGET&gt;
>              &lt;/GROUP&gt;
>           &lt;/FEATURE&gt;
>           ...
>       &lt;/SEGMENT&gt;
>       &lt;/GFF&gt;
>
>     &lt;/DASGFF&gt;

<dl>
<dt>
&lt;!DOCTYPE&gt; (required; one only)

<dd>
<font color="red">Doctype should now be XSD not DTD.</font>The doctype
indicates which formal DTD specification to use.

`     For the features query, the doctype DTD is "`[`http://www.biodas.org/dtd/dasgff.dtd`](http://www.biodas.org/dtd/dasgff.dtd)`".`  
`     `

<dt>
&lt;DASGFF&gt; (required; one only)

<dd>
The appropriate doctype and root tag is DASGFF.

<dt>
&lt;GFF&gt; (required; one only)

<dd>
There is a single &lt;GFF&gt; tag. Its <b>version</b> (required)

`     attribute indicates the current version of the XML form of the`  
`     General Feature Format.  The current version is (arbitrarily) 1.0`  
`     The `<b>`href`</b>` (required) attribute`  
`     echoes the URL query that was used to fetch the current document.`  
`     `

<dt>
&lt;SEGMENT&gt; (required; one or more)

<dd>
The &lt;SEGMENT&gt; tag provides information

`   on the reference segment coordinate system.  `  
`   The `<b>`id`</b>`, `<b>`start`</b>` and `<b>`stop`</b>

`     attributes indicate the position of the segment.  The `<b>`version`</b>  
`     attribute indicates the current version of the sequence map.  The`  
`     id, start, stop, and version attributes are required.  The optional`  
`     `<b>`label`</b>` attribute provides a human readable label for display purposes.`  
`     The optional `<t>`type`</b>` attribute describes the segment`  
`     type, for future compatibility with Sequence Ontology-based feature typing.`  
`     `

<dt>
&lt;FEATURE&gt; (optional; zero or more per SEGMENT)

<dd>
There are zero or more &lt;FEATURE&gt; tags per &lt;SEGMENT&gt;,

`     each providing information on one annotation.  The `  
`     `<b>`id`</b>` attribute (required) is a unique identifier for the feature.  `  
`     It can be used as a reference point for further navigation.`  
`     The `<b>`label`</b>` attribute (optional) is a suggested label to`  
`     display for the feature.  If not present, the `<b>`id`</b>` attribute can be`  
`     used instead.`  
`     `

<dt>
&lt;TYPE&gt; (required; one per FEATURE)

<dd>
Each feature has just one &lt;TYPE&gt; field, which indicates the type
of the

`     annotation.  The attributes are `<b>`id`</b>` (required), which is a unique id for the`  
`     annotation type and can be used to retrieve further information from the`  
`     annotation server (see `<a href="#feature_linking">`Linking to a Feature`</a>`), and`  
`     the `<b>`category`</b>` (optional, recommended)  attribute, which provides functional grouping`  
`     to related types.`  
`     `

`    `<font color="red">`Remove this whole section???  The reference server's annotations can consist of `  
`    additional overlapping landmarks (parents, children, and neighbors),`  
`    which should be marked "yes" in the third attribute `<b>`reference`</b>` `  
`    (optional, defaults to "no") to indicate that the feature is a `  
`    structural landmark within the map (this feature can be annotated).`  
`     The tag contents (optional) is a human readable label for`  
`     display purposes.`  
`     `

`     If a `<b>`reference`</b>` annotation has either or both of the optional attributes,`  
`     `<b>`subparts="yes"`</b>` and `<b>`superparts="yes"`</b>`, then in`  
`     addition to being useable as a`  
`     reference sequence, the feature contains subparts and/or`  
`     superparts that themselves can act as reference features.  This can be used to reconstruct`  
`     reference server's assembly.  See also `<a
      href="#fetching_assembly">`Fetching Assembly Information`</a>`.`  
`     `</font>  
`     `

<dt>
&lt;METHOD&gt; (required; one per FEATURE)

<dd>
Each feature has one &lt;METHOD&gt; field, which identifies the method
used

`     to identify the feature.  The `<b>`id`</b>` (optional) tag can be used to retrieve further information`  
`     from the annotation server.  The tag contents (optional) is a human readable label.  `  
`     `

<dt>
&lt;START&gt;, &lt;END&gt; (<font color="red">Now Optional</font>; one
apiece per FEATURE)

<dd>
These tags indicate the start and end of the feature in the coordinate
system of

`     the reference sequence given in the <SEGMENT> tag.  The `  
`     relationship between the feature start and stop positions and`  
`     the segment start and stop is that the two spans are guaranteed to overlap.`  
`     `

<dt>
&lt;SCORE&gt; (<font color="red">Now Optional</font>; one per FEATURE)

<dd>
This is a floating point number indicating the "score" of the method
used to find

`     the current feature.  The number can only be understood in`  
`     the context of information retrieved from the server by linking to the method.`  
`     If this field is inapplicable, the contents of the tag can be`  
`     replaced with a `<b>`-`</b>` symbol.`  
`     `

<dt>
&lt;ORIENTATION&gt; (<font color="red">Now Optional</font>; one per
FEATURE)

<dd>
This tag indicates the orientation of the feature relative to the
direction of

`     transcription.  It may be `<b>`0`</b>` for features that are unrelated to transcription,`  
`     `<b>`+`</b>`, for features that are on the sense strand, and `<b>`-`</b>`, for features on the`  
`     antisense strand.`  
`     `

<dt>
&lt;PHASE&gt; (<font color="red">Now Optional</font>; one per FEATURE)

<dd>
This tag indicates the position of the feature relative to open reading
frame, if

`     any.  It may be one of the integers `<b>`0`</b>`, `<b>`1`</b>` or`  
`     `<b>`2`</b>`, corresponding to each of the three reading frames, or `<b>`-`</b>` if the`  
`     feature is unrelated to a reading frame.`  
`     `

<dt>
&lt;NOTE&gt; (optional; zero or more per FEATURE)

<dd>
A human-readable note in plain text format.

<dt>
&lt;LINK&gt; (optional; zero or more per FEATURE)

<dd>
A link to a web page somewhere that provides more information about this
feature. The

`     `<b>`href`</b>` (required) attribute provides the URL target for the link. The`  
`   link text is an optional human readable label for display `  
`   purposes.`  
`     `

<dt>
&lt;TARGET&gt; (optional; zero or more per FEATURE)

<dd>
The target sequence in a sequence similarity match. The <b>id</b>
attribute provides the

`     reference ID for the target sequence, and the `<b>`start`</b>` and `<b>`stop`</b>` attributes`  
`     indicate the segment that matched across the target sequence.  All`  
`   three attributes are required.`  
`   More information on the`  
`     target can be retrieved by linking back to the annotation`  
`     server.  See `<a href="#feature_linking">`Linking to a Feature`</a>`.`  
`     `

<dt>
&lt;GROUP&gt; (optional; zero or more per FEATURE)

<dd>
The &lt;GROUP&gt; section is slightly odd, as it is derived from an
overloaded

`     field in the GFF flat file format.  It provides a unique "group" ID that indicates`  
`     when certain features are related to each other.  The canonical`  
`     example is the CDS, exons and introns of a transcribed gene, which logically`  
`     belong together.`

`     The group `<b>`id`</b>` attribute (required) provides an identifier that`  
`     should be used by the client to group features together visually.  Unlike other`  
`     IDs in this protocol, the group ID cannot be used as a database handle to retrieve`  
`     further information about the group. Such information can,`  
`     however, be provided within <GROUP> section, which may contain up`  
`     to three optional tags.`  
`     `

`     The `<b>`label`</b>` attribute (optional) provides a human-readable string that can be`  
`     used in graphical representations to label the glyph.`  
`     `

`     The `<b>`type`</b>` attribute (optional) provides a type ID for the group as a whole,`  
`     for example "transcript".  This ID can be used as a key into the`  
`     `<a href="#stylesheet">`stylesheet`</a>` to select the glyph and graphical characteristics for the`  
`     group as a whole.`  
`     `

<dl>
<dt>
&lt;NOTE&gt; (optional; zero or more per GROUP)

<dd>
A human-readable note in plain text format.

<dt>
&lt;LINK&gt; (optional; zero or more per GROUP)

<dd>
A link to a web page somewhere that provides more information about this
group. The

`     `<b>`href`</b>` (required) attribute provides the URL target for the link. The`  
`   link text is an optional human readable label for display purposes.`  
`     `

<dt>
&lt;TARGET&gt; (optional; zero or more per GROUP)

<dd>
The target sequence in a sequence similarity match. The <b>id</b>
attribute provides the

`     reference ID for the target sequence, and the `<b>`start`</b>` and `<b>`stop`</b>` attributes`  
`     indicate the segment that matched across the target sequence.  All`  
`     three attributes are required.  NOTE: although this tag is present in the GROUP section,`  
`     it applies to the FEATURE, and it is preferred to place it directly in the <FEATURE>`

`     section. Earlier versions of this specification placed the`  
`     TARGET tag in the GROUP section, and clients must recognize and`  
`     accomodate this.`  
`     `

</dl>
</dl>
<table bgcolor="#DEB887" border="1">
<tr>
<th>
New in version 1.5: Exception Handling for Invalid Segments

</th>
</tr>
<tr>
<td>
`     The request for a named segment may fail because: (1) the`  
`     reference sequence is not known to the server or (2) the`  
`     requested region is outside the bounds of the segment.  In both`  
`     cases, an exception is indicated.`  
`     `

`     In the case of a reference server, which is expected to be`  
`     authoritative for the map, the <GFF> section will flag the`  
`     problem by issuing an <ERRORSEGMENT> tag instead of the`  
`     usual <SEGMENT> tag.  This tag has the following format:`  
`     `

>     &lt;ERRORSEGMENT id="id" start="start" stop="stop"&gt;

`     The `<b>`id`</b>` attribute (required) corresponds to the ID of the`  
`     requested segment, and `<b>`start`</b>` and `<b>`stop`</b>` (optional)`  
`     correspond to the requested bounds of the segment if this was`  
`     specified in the request.`  
`     `

`     Unlike a reference server, an annotation server is not required`  
`     to know the identities of all the segments.  Therefore when`  
`     presented with a segment ID that it doesn't recognize, it can't`  
`     know whether this is a true client error or merely an unannotated segment.`  
`     In this case, an annotation server will issue an`  
`     <UNKNOWNSEGMENT> tag.  This tag has the same syntax as`  
`     <ERRORSEGMENT> but doesn't necessarily imply an error.`  
`     `

`     If an annotation server detects a request for a region outside`  
`     the bounds of a segment that it has annotated, it will issue an`  
`     <ERRORSEGMENT> exception.`  
`     `

`     In the case of a request for multiple segments, the server will`  
`     return a mixture of <SEGMENT> sections for valid segments,`  
`     and <ERRORSEGMENT> or <UNKNOWNSEGMENT> sections for`  
`     invalid ones.`  
` `

</td>
</tr>
</table>
<hr>
<h3>
<font color="red">Linking to a Feature has now been deprecated, this
section no longer exists</a></font>

</h3>
<hr>
<h3>
<a name="retrieving_stylesheet">Retrieving the Stylesheet</a>

</h3>
<b>Scope:</b> Annotation servers.

<b>Command:</b> <i>stylesheet</i>

<b>Format:</b>

>     <i>PREFIX</i>/das/<i>DSN</i>/stylesheet

<b>Description:</b> This query can be issued to an annotation server in
order to retrieve the server's recommendations on formatting annotations
retrieved from it. These recommendations are not normative. A viewer is
free to use any display format it chooses.

<b>Arguments:</b> None.

<h4>
<a name="stylesheet">Response:</a>

</h4>
This document is intended to provide hints to the annotation display
client. It maps feature categories and individual types to a series of
glyphs known to the display client.

<b>Format:</b>

>     &lt;?xml version="1.0" standalone="no"?&gt;
>
>     &lt;!DOCTYPE DASSTYLE SYSTEM "http://www.biodas.org/dtd/dasstyle.dtd"&gt;
>     &lt;DASSTYLE&gt;
>       &lt;STYLESHEET version="X.XX"&gt;
>
>          &lt;CATEGORY id="default"&gt;
>              &lt;TYPE id="default"&gt;
>            &lt;GLYPH zoom="high"&gt;
>
>                  &lt;<i>ID</i>&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>                ...
>                  &lt;/<i>ID</i>&gt;
>
>            &lt;/GLYPH&gt;
>            &lt;GLYPH zoom="medium"&gt;
>                  &lt;<i>ID</i>&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>
>                ...
>                  &lt;/<i>ID</i>&gt;
>            &lt;/GLYPH&gt;
>            &lt;GLYPH zoom="low"&gt;
>                  &lt;<i>ID</i>&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>                ...
>                  &lt;/<i>ID</i>&gt;
>            &lt;/GLYPH&gt;
>              &lt;/TYPE&gt;
>          &lt;/CATEGORY&gt;
>
>          &lt;CATEGORY id="group"&gt;
>              &lt;TYPE id="group_id1"&gt;
>            &lt;GLYPH zoom="high"&gt;
>                  &lt;<i>ID</i>&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>                ...
>                  &lt;/<i>ID</i>&gt;
>            &lt;/GLYPH&gt;
>                ...
>          &lt;/CATEGORY&gt;
>
>
>          &lt;CATEGORY id="<i>category1</i>"&gt;
>              &lt;TYPE id="<i>default</i>"&gt;
>            &lt;GLYPH&gt;
>                  &lt;<i>ID</i>&gt;
>
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>                ...
>                  &lt;/<i>ID</i>&gt;
>            &lt;/GLYPH&gt;
>              &lt;/TYPE&gt;
>              &lt;TYPE id="<i>type1</i>"&gt;
>
>            &lt;GLYPH&gt;
>                  &lt;<i>ID</i>&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>                ...
>                  &lt;/<i>ID</i>&gt;
>            &lt;/GLYPH&gt;
>
>              &lt;/TYPE&gt;
>              &lt;TYPE id="<i>type2</i>"&gt;
>            &lt;GLYPH&gt;
>                  &lt;<i>ID</i>&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>
>                ...
>                  &lt;/<i>ID</i>&gt;
>            &lt;/GLYPH&gt;
>              &lt;/TYPE&gt;
>              ...
>          &lt;/CATEGORY&gt;
>
>          &lt;CATEGORY id="<i>category2</i>"&gt;
>
>              &lt;TYPE id="<i>default</i>"&gt;
>            &lt;GLYPH&gt;
>                  &lt;<i>ID</i>&gt;
>                &lt;<i>ATTR</i>&gt;<i>value</i>&lt;/ATTR&gt;
>                ...
>                  &lt;/<i>ID</i>&gt;
>
>            &lt;/GLYPH&gt;
>         &lt;/TYPE&gt;
>              ...
>          &lt;/CATEGORY&gt;
>          ...
>
>     &lt;/STYLESHEET&gt;
>     &lt;/DASSTYLE&gt;

<dl>
<dt>
&lt;!DOCTYPE&gt; (required; one only)

<dd>
The doctype indicates which formal DTD specification to use.

`     For the stylesheet query, the doctype DTD is "`[`http://www.biodas.org/dtd/dasstyle.dtd`](http://www.biodas.org/dtd/dasstyle.dtd)`".`  
`     `

<dt>
&lt;DASSTYLE&gt; (required; one only)

<dd>
The appropriate doctype and root tag is DASSTYLE.

<dt>
&lt;STYLESHEET&gt; (required; one only)

<dd>
There is a single &lt;STYLESHEET&gt; tag. Its <b>version </b>

`     (required)`  
`     attribute indicates the current version of the stylesheet, and`  
`     can be used for caching purposes.`  
`     `

<dt>
&lt;CATEGORY&gt; (required; one or more)

<dd>
There are one or more &lt;CATEGORY&gt; tags, each providing information

`     on the display of a high-level feature category.  The `<b>`id`</b>  
`   (required) `  
`     tag uniquely names the category.  A special name is "default", which`  
`     tells the annotation viewer what format to use for categories that`  
`     are not otherwise specified in the stylesheet.  Another special name`  
`     is "group".  A "group" entry indicates the format to use for`  
`     a particular group of features.`  
`   `  
`     `

<dl>
<dt>
&lt;TYPE&gt; (required; one or more per CATEGORY)

<dd>
There are one or more &lt;TYPE&gt; tags per &lt;CATEGORY&gt;,

`       each providing display suggestions for one type of annotation.  `  
`       The `<b>`id`</b>` (required) uniquely identifies the type.  A special `  
`       id is "default", which, if present, identifies a default style `  
`       for the enclosing category.`  
`       `

<dl>
<dt>
&lt;GLYPH&gt; (required; one or more per TYPE)

<dd>
There is one or more &lt;GLYPH&gt; tag per &lt;TYPE&gt;. It provides

`         information on what glyph (graphical widget) to use to display `  
`         the indicated annotation type.  The optional `<b>`zoom`</b>` attribute,`  
`         implements a simple form of semantic zooming, and allows the client`  
`         to select the glyph and its attributes based on the zoom level.  Possible`  
`         values are "high", "medium" and "low".  If multiple <GLYPH> tags`  
`         are present, this attribute `<b>`must`</b>` be present in order to select`  
`         among them.  A "high" zoom means that there are fewer base pairs per`  
`         pixel (high magnification).  A "low" zoom shows more base pairs.  "Medium" is`  
`         intermediate.  It is left to the client to`  
`         determine the boundaries for "high", "medium" and "low", since this is a`  
`         function of the graphics rendering.`  
`         `

<dl>
<dt>
&lt;<i>ID</i>&gt; (required; one per GLYPH)

<dd>
The ID value refers to a recognized glyph from the glyph types

`           list (`<a href="#glyphid" target="result">`see below`</a>`). `  
`           `

<dt>
&lt;<i>ATTR</i>&gt; (optional; one or more per ID)

<dd>
The recognized ATTR (attributes) are determined by which glyph

`           ID is specified.  See the `<a href="#glyphid">`glyph types`</a>` `  
`           list below for more information.`  
`         `

</dl>
</dl>
</dl>
</dl>
Here is a short stylesheet example:

                ...
         &lt;CATEGORY id="Similarity"&gt;
        &lt;TYPE id="default"&gt;
             &lt;GLYPH&gt;

                      &lt;LINE&gt;
                   &lt;FGCOLOR&gt;gray&lt;/FGCOLOR&gt;
                      &lt;/LINE&gt;
             &lt;/GLYPH&gt;
        &lt;/TYPE&gt;

        &lt;TYPE id="NN"&gt;
             &lt;GLYPH &gt;
                      &lt;BOX&gt;
                   &lt;HEIGHT&gt;4&lt;/HEIGHT&gt;
                   &lt;FGCOLOR&gt;black&lt;/FGCOLOR&gt; 
                   &lt;BGCOLOR&gt;red&lt;/BGCOLOR&gt;

                      &lt;/BOX&gt;
             &lt;/GLYPH&gt;
        &lt;/TYPE&gt;
        &lt;TYPE id="NP"&gt;
             &lt;GLYPH&gt;
                      &lt;TOOMANY&gt;

                   &lt;HEIGHT&gt;4&lt;/HEIGHT&gt;
                   &lt;FGCOLOR&gt;black&lt;/FGCOLOR&gt;
                   &lt;BGCOLOR&gt;blue&lt;/BGCOLOR&gt;
                      &lt;/TOOMANY&gt;

             &lt;/GLYPH&gt;
        &lt;/TYPE&gt;
        &lt;TYPE id="PN"&gt;
             &lt;GLYPH&gt;
                      &lt;BOX&gt;
                   &lt;HEIGHT&gt;3&lt;/HEIGHT&gt;

                   &lt;FGCOLOR&gt;blue&lt;/FGCOLOR&gt;
                   &lt;BGCOLOR&gt;green&lt;/BGCOLOR&gt;
                      &lt;/BOX&gt;
             &lt;/GLYPH&gt;
        &lt;/TYPE&gt;

        &lt;TYPE id="PP"&gt;
             &lt;GLYPH&gt;
                      &lt;SPAN&gt;
                   &lt;HEIGHT&gt;4&lt;/HEIGHT&gt;
                   &lt;FGCOLOR&gt;gray&lt;/FGCOLOR&gt;

                      &lt;/SPAN&gt;
             &lt;/GLYPH&gt;
        &lt;/TYPE&gt;
         &lt;/CATEGORY&gt;
          ...
          

Groups can also have stylesheet entries. If present, they are located in
the category named "group". Typically a group will be associated with
the "line" glyph, which as described below, draws connections between
the members of a group.

A sample stylesheet used for the WormBase DAS server can be found at <a
href="sample_stylesheet.xml"><http://www.biodas.org/documents/sample_stylesheet.xml></a>.

<h3>
Glyphs and Groups

</h3>
Glyphs and their attributes are typically applied to individual
features. However, they can be applied to entire groups as well (via the
&lt;GROUP&gt; <b>type</b> attribute). In this case, the glyph will apply
to the connecting regions <b>between</b> the components of the group.

For example, to indicate that the exons in a "transcript" group should
be drawn with a yellow box, that the utrs should be drawn with a blue
box, and that the connections between exons should be drawn with a
hat-shaped line:

    &lt;CATEGORY id="Transcription"&gt;
       &lt;TYPE id="exon"&gt;
          &lt;GLYPH&gt;
             &lt;BOX&gt;
                &lt;BGCOLOR&gt;yellow&lt;/BGCOLOR&gt;

             &lt;/BOX&gt;
          &lt;/GLYPH&gt;
       &lt;/TYPE&gt;

       &lt;TYPE id="utr"&gt;
          &lt;GLYPH&gt;
             &lt;BOX&gt;

                &lt;BGCOLOR&gt;blue&lt;/BGCOLOR&gt;
             &lt;/BOX&gt;
          &lt;/GLYPH&gt;
       &lt;/TYPE&gt;
    &lt;/CATEGORY&gt;

    &lt;CATEGORY id="group"&gt;
    &lt;TYPE id="transcript"&gt;
       &lt;GLYPH&gt;
          &lt;LINE&gt;
             &lt;FGCOLOR&gt;black&lt;/FGCOLOR&gt;
             &lt;LINE_STYLE&gt;hat&lt;/LINE_STYLE&gt;

          &lt;/LINE&gt;
       &lt;/GLYPH&gt;
    &lt;/TYPE&gt;
    ...

<hr>
<font color="red">

<h2>
<a name="assemblies">Fetching Sequence Assemblies has been deprecated
due to a percieved lack of use - so this secion no longer exists</a>

</h2>
</font>

<hr>
<h2>
Feature Types and Categories

</h2>
<font color="red">Maybe useful to put SO equivalents for most of these
examples???</font>

This is a list of generic feature categories and specific feature types
within them. This list was derived from the features currently exported
by ACeDB/GFF and is not comprehensive. Suggestions for modifications,
additions and deletions are welcomed.

<h3>
<cite>component</cite>

</h3>
This category indicates that the feature is a child component of the
reference sequence in the current assembly. When combined with the
<b>reference="yes"</b> attribute, this indicates that the feature can be
used as a reference point to retrieve subfeatures contained within it
(including subcomponents).

<h3>
<cite>supercomponent</cite>

</h3>
This category indicates that the feature is the parent of the reference
sequence in the current assembly. When combined with the
<b>reference="yes"</b> attribute, this indicates that the feature can be
used as a reference point to retrieve features that completely contain
the selected range of the reference sequence.

<h3>
<cite>translation</cite>

</h3>
The <cite>translation</cite> category is used for features that relate
to regions of the sequence that are translated into proteins. Features
that relate to transcription are separate (see below).

Features:

-   stop - position of the translation stop codon
-   ATG - position of the start codon
-   CDS - position of the coding region
-   5'UTR - untranslated region
-   3'UTR - untranslated region
-   misc\_translated - miscellaneous

It is recommended, but not required, that the &lt;FEATURE&gt; section
contain &lt;LINK&gt; and/or &lt;NOTE&gt; tags that provide further
information on the transcription feature.

<h3>
<cite>transcription</cite>

</h3>
The <cite>transcription</cite> category is used for features that relate
to regions of the sequence that are transcribed into RNA.

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

It is recommended, but not required, that the &lt;FEATURE&gt; section
contain &lt;LINK&gt; and/or &lt;NOTE&gt; tags that provide further
information on the transcription feature.

<h3>
<cite>variation</cite>

</h3>
The <cite>variation</cite> category is used for features that relate to
regions of the sequence that are polymorphic.

Features:

-   insertion
-   deletion
-   substitution
-   misc\_variation

It is recommended, but not required, that the &lt;FEATURE&gt; section
contain &lt;LINK&gt; and/or &lt;NOTE&gt; tags that provide further
information on the variation.

<h3>
<cite>structural</cite>

</h3>
The <cite>structural</cite> category is used for features that relate to
mapping, sequencing and assembly, as well as for various landmarks that
carry no intrinsic biological information.

Features:

-   clone
-   primer\_left
-   primer\_right
-   oligo
-   assembly\_tag
-   misc\_structural

It is recommended, but not required, that the &lt;FEATURE&gt; section
contain &lt;LINK&gt; and/or &lt;NOTE&gt; tags that provide further
information on the structural feature.

<h3>
<cite>similarity</cite>

</h3>
The <cite>similarity</cite> category is used for areas that are similar
to other sequences. Similarity features should have a &lt;METHOD&gt; tag
that indicates the algorithm used for the sequence comparison, and a
&lt;TARGET&gt; tag that indicates the target of the match.

Features:

-   NN -- nucleotide to nucleotide similarity (e.g. blastn)
-   NP -- nucleotide to protein similarity (e.g. blastx)
-   PN -- protein to nucleotide similarity (e.g. tblastn)
-   PP -- protein to protein similarity (e.g. tblastx)
-   misc\_homology

<h3>
<cite>repeat</cite>

</h3>
The <cite>repeat</cite> category is used for areas that contain
repetitive DNA. This category is used both for low-complexity regions,
such as microsatellites, and for more biologically interesting features,
such as transposon insertion sites.

Features:

-   microsatellite
-   inverted
-   tandem
-   transposable\_element
-   LINE - long repeat not definitely identified as a transposon
-   misc\_repeat

It is recommended, but not required, that the &lt;FEATURE&gt; section
contain &lt;LINK&gt; and/or &lt;NOTE&gt; tags that provide further
information on the repetitive element.

<h3>
<cite>experimental</cite>

</h3>
The <cite>experimental</cite> category is a catchall used to flag areas
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

It is recommended, but not required, that the &lt;FEATURE&gt; section
contain &lt;LINK&gt; and/or &lt;NOTE&gt; tags that provide further
information on the nature of the experimental data.

<hr>
<a name="glyphid">

<h2>
Glyph Types

</h2>
This section describes a set of generic "glyphs" that can be used by
sequence display programs to display the position of features on a
sequence map. The annotation server may use these glyphs to send display
suggestions to the viewer via the <a
href="#stylesheet">stylesheet document</a>.

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

<dl>
<dt>
INT

<dd>
An integer

<dt>
FLOAT

<dd>
A floating point number (not currently used)

<dt>
STRING

<dd>
A text string

<dt>
COLOR

<dd>
A color. Colors can be specified using the "\#RRGGBB" format

`     commonly used in HTML, or as one of the 16 IBM VGA colors`  
`     recognized by Netscape and Internet Explorer.`  
` `

<dt>
BOOL

<dd>
A boolean value, either "yes" or "no".

<dt>
FONT

<dd>
A font. Any of the font identifiers recognized by Web browsers

`     is acceptable, e.g. "helvetica".`  
` `

<dt>
FONT\_STYLE

<dd>
One of "bold", "italic", "underline".

<dt>
LINE\_STYLE

<dd>
One of "hat", "solid", "dashed".

</dl>
Some attributes are shared by all glyphs. Others are glyph-specific. The
following attributes are shared in common:

<dl>
<dt>
HEIGHT

<dd>
type: INT

<dd>
The height of the glyph, in pixels. For the text font, this is

`     equivalent to the FONTSIZE attribute.`  
` `

<dt>
FGCOLOR

<dd>
type: COLOR

<dd>
The foreground color of the glyph. This is the line and outline color
for graphical

`     glyphs, and the font color for text glyphs.`  
` `

<dt>
BGCOLOR

<dd>
type: COLOR

<dd>
The background color of the glyph. For hollow glyphs, such as boxes,
this is the color

`     of the interior of the box.  For solid glyphs, such as text, this is ignored`  
` `

<dt>
LABEL

<dd>
BOOL

<dd>
Whether the glyph should be labeled with its name, as dictated

`     by the <FEATURE> `<b>`label`</b>` attribute in the DASGFF document.`  
` `

<dt>
BUMP

<dd>
BOOL

<dd>
Whether the glyph should "bump" intersecting glyphs so that they

`     do not overlap.`

</dl>
<h3>
<cite>ARROW</cite>

</h3>
A double-headed arrow with an axis either orthogonal or parallel to the
sequence map.

Attributes:

<dl>
<dt>
PARALLEL

<dd>
type: BOOL

<dd>
Arrows run either parallel ("yes") or orthogonal("no") to the

`     sequence axis.`

</dl>
<h3>
<cite>ANCHORED\_ARROW</cite>

</h3>
An arrow that has an arrowhead at one end, and an "anchor" (typically a
diamond or line) at the other. The arrow points in the direction
indicated by the &lt;ORIENTATION&gt; tag.

Attributes:

<dl>
<dt>
PARALLEL

<dd>
type: BOOL

<dd>
Arrows run either parallel ("yes") or orthogonal("no") to the

`     sequence axis.`

</dl>
<h3>
<cite>BOX</cite>

</h3>
A rectangular box.

Attributes:

<dl>
<dt>
LINEWIDTH

<dd>
type: INT

<dd>
Width of the box outline.

</dl>
<h3>
<cite>CROSS</cite>

</h3>
A cross "+". Common used for point mutations and other point-like
features.

Attributes:

(no glyph-specific attributes)

<h3>
<cite>DOT</cite>

</h3>
A dot. Common used for point mutations and other point-like features.

Attributes:

(no glyph-specific attributes)

<h3>
<cite>EX</cite>

</h3>
"X" marks the spot. Common used for point mutations and other point-like
features.

Attributes:

(no glyph-specific attributes)

<h3>
<cite>HIDDEN</cite>

</h3>
A feature that is invisible, intended to support semantic zooming
schemes in which a feature is hidden at particular zooms.

Attributes: none.

<h3>
<cite>LINE</cite>

</h3>
A line. Lines are equivalent to arrows with both the
<cite>northeast</cite> and <cite>southwest</cite> attributes set to
"no".

Attributes:

<dl>
<dt>
STYLE

<dd>
type: LINE\_STYLE

<dd>
The line type. A type of "hat" draws an inverted V

`     (commonly used for introns).  A type of "solid"`  
`     draws a horizontal solid line in the indicated color.  A type of`  
`     "dashed" draws a dashed horizonal line in the indicated color.`

</dl>
<h3>
<cite>SPAN</cite>

</h3>
A spanning region, the recommended representation is a horizontal line
with vertical lines at each end.

Attributes:

(no glyph-specific attributes)

<h3>
<cite>TEXT</cite>

</h3>
A bit of text.

Attributes:

<dl>
<dt>
FONT

<dd>
type: FONT

<dd>
The font.

<dt>
FONTSIZE

<dd>
type: INT

<dd>
The font size.

<dt>
STRING

<dd>
type: STRING

<dd>
The text to render.

<dt>
STYLE

<dd>
type: FONT\_SYTLE

<dd>
The style in which to render this glyph. Multiple FONT\_STYLE

`     attributes may be present.`

</dl>
<h3>
<cite>PRIMERS</cite>

</h3>
Two inward-pointing arrows connected by a line of a different color.
Used for showing primer pairs and a PCR product. The length of the
arrows is meaningless.

There are no glyph-specific attributes, but in this context the
foreground color is the color of the arrows, and the background color is
the color of the line that connects them.

<h3>
<cite>TOOMANY</cite>

</h3>
Too many features than can be shown. Recommended for use in
consolidating sequence homology hits. The recommended visual
presentation is a set of overlapping boxes.

Attributes:

<dl>
<dt>
LINEWIDTH

<dd>
type: INT

<dd>
Width of the glyph.

</dl>
<h3>
<cite>TRIANGLE</cite>

</h3>
A triangle. Commonly used for point mutations and other point-like
features. The triangle is always drawn in the center of its range, but
its width and height can be controlled by HEIGHT and LINEWIDTH
respectively.

Attributes:

<dl>
<dt>
LINEWIDTH

<dd>
type: INT

<dd>
Width of the glyph.

<dt>
DIRECTION

<dd>
One of "N", "E", "S", and "W"

</dl>
<hr>
<h2>
Other Issues

</h2>
The distributed annotation system must have a mechanism for detecting
and resolving version skew across reference and annotation servers.
Although one such mechanism is currently incorporated into the
ACeDB-based prototype, it is largely untested and hence not yet a part
of the DAS standard.

<h2>
<a name="changes">Changes</a>

</h2>
This section was added at version 0.99.

<h3>
Version 1.51

</h3>
1.  The description of the entry\_points document was out of synch
    `     with the DTD.  Also there seems to have been some semantic drift between`  
    `     Dazzle, the UCSC server, and LDAS with regards to the attributes of the`  
    `     <SEGMENT> tag.  This has now been made explicit, and the DTD relaxed`  
    `     to allow all styles.`

<h3>
Version 1.5

</h3>
1.  Added capabilities header.
2.  Added exception handling for invalid sequence IDs.
3.  Added feature\_id request.
4.  Corrected syntax errors in stylesheet example.

<h3>
Version 1.01

</h3>
1.  Split assembly functionality into "component" and "supercomponent".
2.  Removed redundant descriptions of glyph attributes.

<h3>
Version 1.0

</h3>
1.  Removed deprecated <i>resolve</i> command.
2.  Removed deprecated <i>entry\_points</i> <b>ref</b> argument.
3.  Added <b>superparts</b> attribute to DASGFF &lt;FEATURE&gt; tag.
4.  New discussion of how to move upwards in an assembly.
5.  Reorganized specification to put responses close to requests.
6.  Added a stylesheet example document.
7.  Normalized the names of glyph COLOR and FILLCOLOR attributes to
    `     FGCOLOR and BGCOLOR.`  
    ` `

8.  Added the LABEL attribute to all glyphs.
9.  Added the STYLE attribute to the LINE glyph.
10. Added the ability to assign a glyph to a group.
11. Added HIDDEN glyph.

<h3>
Version 0.999

</h3>
1.  Added LINK, NOTE, and TARGET to FEATURE
2.  Added section entitled "Fetching Sequence Assemblies"

<h3>
Version 0.998

</h3>
1.  Deprecated regular expression matching for types and categories.
2.  Allow multiple TYPE arguments for logical OR filtering.
3.  Made FEATURE optional within features return document.
4.  Made TYPE optional within types return document.

<h3>
Version 0.996

</h3>
1.  Added subparts tag to features and entry\_points.
2.  Removed the requirement that the server return features that
    `   do not overlap with the requested segment.`  
    ` `

3.  Added support for multiple segments/sequences in types document.

<h3>
Version 0.995

</h3>
1.  Added support for multiple segments/sequences in returned documents.
2.  Added support for assembly components.

<h3>
Version 0.99

</h3>
1.  Allow query parameters to be POSTed to the DAS URL.
2.  Added compatibility warning about SOAP conversion.
3.  Use Version 8 regular expressions rather than GNU's, giving
    `     compatibility with both Perl regex and GNU regex.`  
    ` `

4.  Made the <b>id</b> attribute of the &lt;TYPE&gt; tag required.
5.  Changed the WIDTH glyph attribute to HEIGHT throughout.

<hr>
<address>
Lincoln D. Stein, lstein@cshl.org  
<a href="http://www.cshl.org/">Cold Spring Harbor Laboratory</a>

</address>
Last modified: Thu May 22 12:55:39 EDT 2003