---
title: DAS1.6E
permalink: wiki/DAS1.6E/
layout: wiki
---

Details of existing and proposed Extensions to the [DAS 1.6
specification](/wiki/DAS1.6 "wikilink").

Existing Extensions
===================

These extensions have undergone development and have solid
implementations.

Alignment
---------

This extension is a new command, carried forward from its equivalent
extension in the [DAS 1.53E extended
specification](http://www.dasregistry.org/spec_1.53E.jsp) **with some
modifications**. An official specification for the extension under DAS
1.6 was proposed, but was removed in draft 7. Instead a redesign of the
query mechanism is proposed in order to better accommodate genomic
alignments. For posterity, draft 6 contains the last version before the
specification was removed (see the [DAS 1.6](/wiki/DAS1.6 "wikilink") page for
details of drafts). Discussion should be directed to the mailing list
(see the [Community Portal](/wiki/BioDAS:Community_Portal "wikilink") page for
details).

Interaction
-----------

The [DASMI](http://dasmi.de/) extension expands DAS to apply to
molecular interactions. It is part of the [DAS 1.53E
specification](http://www.dasregistry.org/spec_1.53E.jsp) (i.e. is an
existing extension to the [1.53
specification](http://biodas.org/documents/spec.html)). It is detailed
[here](http://www.dasregistry.org/extension_interaction.jsp).

Entry Points for annotation servers
-----------------------------------

**January 28, 2010**

**NOTE:** this modification is **included** in the final
[DAS1.6](/wiki/DAS1.6 "wikilink") specification.

#### Rationale

In DAS version 1.6, the *entry\_points* command is required for
reference servers and optional for annotation servers.

However, it is difficult for annotation servers to support this command
because the start/stop attributes of the SEGMENT element are mandatory.
In contrast to reference servers, annotation servers very rarely know
the length of each entry point and therefore cannot satisfy this
requirement. This is a pity because it'd be useful for clients if
annotation servers were able to provide a list of IDs for the segments
it has annotated. This would be very easy to implement because every
server always knows the IDs of the segments it has annotated, and needs
this information in order to support the *unknown-segment* capability.

#### Required Change

Allowing annotation servers to easily list their entry points would
require amending the specification to say something like:

-   reference servers must always list all possible segments with their
    start/stop positions
-   annotation servers implementing entry\_points must list only the
    segments they have annotated, and start/stop are optional

DAS writeback
-------------

**June 10,2009**

This is a working document and a proposal for an extension to the [DAS
1.6 specification](/wiki/DAS1.6 "wikilink") in order to support writeback
capabilities. A writeback server should have, at least, the methods for
the basic reading/writing operations, which in Databases is recognized
as CRUD(Create, Read, Update and Delete). The reading component is
already solved for the DAS protocol, even is possible to affirm that all
the current commands in the specification are reading commands for
different kind of information.

One or more Writeback servers can be associated to a coordinate system,
however just one coordinate system can be handle for a writeback source,
and in such a way be sure to identify the feature with the correct
segment of the right specie.

DAS is partially following the concepts of RESTful services, and that is
the reason why most of this proposal is inspired in the RESTful concept
of *Uniform Interface*, which means that all the resources should be
manipulated using a predefined set of operations. DAS use HTTP(see
[RFC2616](ftp://ftp.isi.edu/in-notes/rfc2616.txt)) as its communication
protocol, therefore the logic *Uniform Interface* for DAS is the use of
the HTTP methods PUT, GET, POST and DELETE to access the 4 CRUD
operations. Next, is explained the proposed details to use this methods
with DAS, but first a description of the das writeback document.

### DAS Writeback Document

The document used for all the methods is an XML-formatted "DASGFF"
document. All the information of an annotation to create/edit could be
supplied using this format, which implies that the implementation of
this extension is dependent of the DAS version implemented. All the
elements of this format are explained in the [DAS 1.6
specification](/wiki/DAS1.6 "wikilink"). An example of the Document is below,
and as it will be explained later the same format could be used for the
input or the output of one of the HTTP methods. Extra information can be
required depending of the implementation as metadata of the operation.
For those cases the element NOTE should be used in a notation KEY=VALUE.
In the example below this notation is used to represent the credentials
of the user who sent or is sending this feature to the server. <code>

<?xml version="1.0" standalone="no"?>
<DASGFF>  
` `<GFF href="http://some_server">  
`  `<SEGMENT id="P05067" start="1" stop="770" version="7dd43312cd29a262acdc0517230bc5ca" label="Amyloid beta A4 protein" >  
`   `<FEATURE id="new" label="testAnnotation" >  
`    `<TYPE id="SO:0001072" cvId="SO:0001072" >`extramembrane_polypeptide_region`</TYPE>  
`    `<METHOD id="ECO:0000053" cvId="ECO:0000053" >`computational combinatorial analysis`</METHOD>  
`    `<START>`50`</START>  
`    `<END>`100`</END>  
`    `

<NOTE>
Test annotation

</NOTE>
<NOTE>
USER=tester

</NOTE>
<NOTE>
PASSWORD=tester

</NOTE>
`   `</FEATURE>  
`  `</SEGMENT>  
` `</GFF>  
</DASGFF>

</code>

### HTTP METHODS (DAS writeback methods)

#### POST

The HTTP method *POST* should be used to create a feature in the server.
The writeback document should be sent to the server in a POST variable
called ''' *\_content* '''. The server should create an identifier for
this feature. This id should be the URI formed by the concatenation of
the URL of the writeback server and some unique number for the server(a
consecutive number).\`http://writeback/0\` The response should be based
in the HTTP headers of the DAS specification. (i.e. 200 OK, 500 Server
error, etc.). The content of the response should be a GFFDAS format with
the information that was created in the database therefore if everything
is correct the response will be almost the same that the input document.
The only difference will be the meta-information added for the server as
notes, for example

<code>

<NOTE>
USER=tester

</NOTE>
<NOTE>
DATE=2011-03-30 11:42:56.325

</NOTE>
<NOTE>
VERSION=1

</NOTE>
</code>

#### PUT

The behaviour of the method *PUT* is very similar to the described for
*POST*. However in this case the writeback document is the content
itself of the request(i.e. Is not embedded in any parameter). An
important difference is the management of the id, which is defined as
the value in the field *id* of the element *FEATURE* if this value is a
valid URI, otherwise is the URI formed by the base URL in the field
*href* concatenated with the value in the field *id* of the element
*FEATURE*. For example for the same writeback document of above the id
in the element *FEATURE* will be: <code>

<FEATURE id="http://some_server/new" label="testAnnotation">

</code> The response should follow the same rules as the *POST* method.

#### DELETE

The *DELETE* method doesn't require a writeback document for the input,
because in order to DELETE a feature is just necessary to identify it.
The identification of a feature will be reconstructed by its id and the
id of the segment where it will be deleted. The authentication
credentials are also require for this transaction. The format of the
HTTP connection should be: <code>

`DELETE [server]?segmentid=[segmentid]&featureid=[featureid]&user=[user]&password=[password]&href=[href]`

</code>

A successful response SHOULD be 200 (OK) if the response includes an
entity describing the status, 202 (Accepted) if the action has not yet
been enacted, or 204 (No Content) if the action has been enacted but the
response does not include an entity. The content of the response should
be in the same GFF format but the parameter *label* of the element
*FEATURE* should be **DELETED**. The *id* attribute of the elements
*TYPE* and *METHOD* should be also set to **DELETED** because this
attributes are mandatory for a GFF document.: <code>

`     `<FEATURE id="http://writeback/new" label="DELETED">  
`       `<TYPE id="DELETED" />  
`       `<METHOD id="DELETED" />

</code>

#### GET

All the commands of reading in the DAS specification should Apply here,
however the deleted features should be also included here(in the same
format as explained above) and is the client who decide how to used this
information. There is an extra command that a writeback source should
implement:

### Historical Command

*Retrieve all the versions that the writeback has for an specific
feature.*

**Scope:** Writeback servers.

**Command:** *historical*

**Format:**

<code>

*`PREFIX`*`/das/`*`DSN`*`/historical?feature==`*`FEATUREID`*

</code>

<b>Description:</b> This query returns all the versions for an specific
feature embedded in its respective segment(s).

<b>Arguments:</b>

<dl>
<dt>
<b>feature</b> (required; one)

<dd>
URL identifier of a particular feature in the writeback.

</dl>
Here is an example of a valid request:

        http://www.writeback.com/das/writeback/historical?feature=http://writeback/9

#### Response:

The document returned from the *features* request is an XML-formatted
"DASGFF" document.

**Format:**

<code>

<?xml version="1.0" standalone="no"?>
<?xml-stylesheet href="xslt/features.xsl" type="text/xsl"?>
<DASGFF>  
` `<GFF href="http://writeback/historical?feature=http://some_server/new">  
`   `<SEGMENT id="P05067" start="1" stop="770" version="7dd43312cd29a262acdc0517230bc5ca" label="Amyloid beta A4 protein">  
`     `<FEATURE id="http://some_server/new" label="DELETED">  
`       `<TYPE id="DELETED" />  
`       `<METHOD id="DELETED" />  
`       `<SCORE>`0.0`</SCORE>  
`       `

<NOTE>
USER=tester

</NOTE>
<NOTE>
DATE=2011-03-30 15:00:54.623

</NOTE>
<NOTE>
VERSION=3

</NOTE>
`     `</FEATURE>  
`     `<FEATURE id="http://some_server/new" label="testAnnotation">  
`       `<TYPE id="SO:0001072" cvId="SO:0001072">`extramembrane_polypeptide_region`</TYPE>  
`       `<METHOD id="ECO:0000053" cvId="ECO:0000053">`computational combinatorial analysis`</METHOD>  
`       `<START>`50`</START>  
`       `<END>`100`</END>  
`       `

<NOTE>
Test annotation

</NOTE>
<NOTE>
USER=tester

</NOTE>
<NOTE>
DATE=2011-03-30 12:17:17.344

</NOTE>
<NOTE>
VERSION=1

</NOTE>
`     `</FEATURE>  
`     `<FEATURE id="http://some_server/new" label="testAnnotation">  
`       `<TYPE id="SO:0001072" cvId="SO:0001072">`extramembrane_polypeptide_region`</TYPE>  
`       `<METHOD id="ECO:0000053" cvId="ECO:0000053">`computational combinatorial analysis`</METHOD>  
`       `<START>`150`</START>  
`       `<END>`200`</END>  
`       `

<NOTE>
Test annotation 2

</NOTE>
<NOTE>
USER=tester

</NOTE>
<NOTE>
DATE=2011-03-30 14:17:36.53

</NOTE>
<NOTE>
VERSION=2

</NOTE>
`     `</FEATURE>  
`   `</SEGMENT>  
` `</GFF>  
</DASGFF>

</code>

### User Authentication

The control of who is authorized or not to made modifications in the
writeback is a source implementation issue, however the recommendation
to pass parameters is through the NOTE element, in the way
\[KEY\]=\[VALUE\], creating as many NOTE elements as parameters are
required for the particular implementation. <code>

<NOTE>
USER=login

</NOTE>
<NOTE>
PASSWORD=keypass

</NOTE>
</code>

Proposed Extensions
===================

These extensions are merely proposals for future modifications, and do
not yet have implementations.

DAS search
----------

**December 3,2010**

This is a working document and a proposal for an extension to the [DAS
1.6 specification](/wiki/DAS1.6 "wikilink") in order to a mechanism for
programmatic search of content within annotation servers.

This proposal pretends to provide the basis for an optional extension of
the *features* command to support elaborated queries.

### New Argument - query

*query*, a new argument for the *features* command should be added, so
now the request of this command is defined as:

<code>

`SERVER/das/DSN/features?segment=RANGE`  
` [;segment=RANGE]`  
` [;type=TYPE]`  
` [;type=TYPE]`  
` [;category=CATEGORY]`  
` [;category=CATEGORY]`  
` [;feature_id=ID]`  
` [;maxbins=BINS]`  
` [;query=DASQUERY]`

</code>

Where DASQUERY is described below. In the case of a query contains
simultaneously the attributes *segment*, *feature\_id* and *query*. they
should be treated as a disjunction of the filtered subsets (ie. the
result features should pass the 3 conditions).

#### DAS Query Language

Based in
[Lucene](http://lucene.apache.org/java/3_0_0/queryparsersyntax.html)
query language. A query is broken into terms and operators:

-   **Terms**: single words or phrases (group of words surrounded
    by quotes). E.g. polypeptide AND "alpha helix"
-   **Fields**: used to search in a specific column. See the next
    section for the specific field names. E.g. type:membrane
-   **Term modifiers**: wildcard searches, fuzzy searches, proximity and
    range searches. E.g. cur\*
-   **Operands**: OR (or space), AND, NOT, +, -. E.g. typeCvId:CV:00001
    AND featureLabel:"one Feature"
-   **Grouping and field grouping**: (typeCvId:CV:00001 AND
    featureLabel:"one Feature") OR typeId:twoFeatureTypeIdOne

The following table shows the available standard fields that can be used
in DAS advanced searches:

| Field Name   | Searches on                                                                                                                 | Example                                               |
|--------------|-----------------------------------------------------------------------------------------------------------------------------|-------------------------------------------------------|
| featureId    | The Id of the feature                                                                                                       | featureId:IPR003593\_450\_639                         |
| featureLabel | In case the feature has a label                                                                                             | featureLabel:"ABC transporter"                        |
| segmentId    | The Id of the segment                                                                                                       | segmentId:P05701                                      |
| segmentLabel | In case the segment has a label                                                                                             | segmentLabel:P53                                      |
| segmentStart | Using the start coordinate of the segment                                                                                   | start:1000                                            |
| segmentStop  | Using the stop coordinate of the segment                                                                                    | stop:2000                                             |
| typeId       | Using the id of the type associated to the feature                                                                          | typeId:SO\\:0000417                                   |
| typeCvId     | Using(if available) the ontology id of the type associated to the feature                                                   | typeCvId:SO\\:0000417                                 |
| typeLabel    | Using(if available) the label of the type associated to the feature                                                         | typeLabel:polypeptide\_domain                         |
| typeCategory | Using(if available) the category of the type associated to the feature                                                      | typeCategory:coverage                                 |
| type         | Using any of the field related with the type associated to the feature(ie. typeId OR typeCvId OR typeLabel OR typeCategory) | type:SO\\:0000417                                     |
| methodId     | Using the id of the method associated to the feature                                                                        | methodId:ECO\\:0000029                                |
| methodCvId   | Using(if available) the ontology id of the method associated to the feature                                                 | methodCvId:ECO\\:0000029                              |
| methodLabel  | Using(if available) the label of the method associated to the feature                                                       | methodLabel:"inferred from InterPro motif similarity" |
| method       | Using any of the fields of the method associated to the feature                                                             | method:inferred                                       |
| start        | Using the start coordinate of the feature                                                                                   | start:100                                             |
| stop         | Using the stop coordinate of the feature                                                                                    | stop:200                                              |
| score        | Using(if available) the score of the feature                                                                                | score:0.5                                             |
| orientation  | Using(if available) the orientation of the feature                                                                          | orientation:\\+                                       |
| phase        | Using the phase of the feature                                                                                              | phase:1                                               |
| note         | Using(if available) any of the notes of the feature                                                                         | note:200                                              |
| link         | Using(if available) any of the links of the feature                                                                         | link:200                                              |
| target       | Using(if available) any of the targets of the feature                                                                       | target:supercontig201                                 |
| parent       | Using(if available) any of the parents of the feature                                                                       | parent:chromosome1                                    |
| part         | Using(if available) any of the parts of the feature                                                                         | part:contig201a                                       |
| all          | Using any of the above fields of the feature                                                                                | all:chromosome                                        |

### Capability

A source able to deal with this extension have to reflect this in its
*sources* command by a similar line as:

<capability type="das1:advanced-search"   />

### Implementation Notes

As a consequence of the first prototype of the advanced search
capability in myDas, the capabilities *entry\_points* and
*feature-by-id* are requirements of the new capability in order to
automatically create indexes to improve the search performance.

Support for alternative content formats
---------------------------------------

Content negotiation, either via request parameters or HTTP
auto-negotiation. This would allow support for response formats other
than DAS XML. Examples might include JSON, XHTML, etc.

#### Rationale

DAS XML is expressive but can be very verbose. This is particularly
problematic for the features query, which depending on server and query
can result in extremely large responses. It is therefore desirable to
allow more efficient alternative content formats to be returned within
the general DAS query framework, particularly for feature queries.

Another advantage is simplicity. If we allow a DAS server to send back
other formats, then it can take advantage of libraries such as Picard or
tools like samtools to run region queries on indexed flat files and
return the data without having to parse and process them. All the server
would have to do is decode the request URL and then use Picard or
samtools to grab the data from the flat file and return it to the
client.

#### Proposed Implementation

At the DAS workshop the following was discussed as an alternative to the
initial proposal below. The advantage of this alternative is that it
should work well for both UCSC style sources with tracks that are
separated by type and tracks seperated by data source name (e.g.
ensembl). Having the format request and response separate from the types
request/response means we do not break the 1.6 spec. The format as in
this example also allows data providers to allow different response
formats for different commands. It has also been proposed that format
names be put on this wiki so as not to get name clashes. Names also need
to be specific in that for example "JSON" format really does not say
anything about the way the data is set out it is similar to saying my
data comes as a tab separated file - we would not know what the
"columns" represent.

Can query a datasource for a list of the formats it supports for example
../das/hg18/format should result in something like the following:

    <DASFORMAT>
     <COMMAND name="das1:features">
       <FORMAT name="das-JSON">
    ..if no types specified here then all types for this source have this format for this command
         <TYPE id="gene"/>
         <TYPE id="exon"/>
       </FORMAT>
       <FORMAT name="das-GoogleProtocolBuffers">
       ..if no types specified here then all types for this source have this format for this command
         <TYPE id="gene"/>
         <TYPE id="exon"/>
       </FORMAT>
     </COMMAND>
    <COMMAND name="das1:entry_points">
       <FORMAT name="das-JSON">
       </FORMAT>
     </COMMAND>
    </DASFORMAT>

##### Initial proposal

Addition of an optional zero-or-more <FORMAT> element as a child of
<TYPE> element in types command response, with required attributes
"name" (arbitrary character string that uniquely identifies this format
within the server) and "mimetype" (must follow standard [mimetype
identifier rules](http://tools.ietf.org/html/rfc2045)). Note that this
strategy also allows for multiple formats with the same mimetype.

Addition of an optional "format" query parameter to features query.
Format parameter SHOULD be a format name that the server recognizes. If
server can return ALL features that satisfy the feature query in the
specified format, it should do so, and set the response Content-Type
header accordingly. If the server does not recognize the requested
format, or cannot return in that format all of the features that satisfy
the features query, it should return an HTTP error message (with
X-DAS-Status also set?).

Both of these additions are optional, therefore these changes will not
affect servers or clients that do not support alternative content
formats.

Authentication
--------------

Per-user access control for DAS servers would be useful for federated
distribution of private data, and would be necessary for the use of DAS
in circumstances that are commercially sensitive or subject to patient
privacy controls. Although authentication can theoretically be overlaid
onto DAS, it has become apparent that a lack of an explicit
recommendation for a single implementation strategy has prevented
take-up by DAS clients and servers.

### Outcome from the 2012 DAS workshop

There are two authentication scenarios:

-   **private data** The entry point/data source contains only
    private data. Any access to data and capabilities of that source
    *require* the client to present some credentials. This is analagous
    to realm based HTTP Authentication.
-   **mixed mode** The response from the data source is dependent on the
    authentication state of the user. The source can provide some public
    data to unauthenticated users but may present additional data or
    capabilites to authenticated clients. This is equivalent to
    application implemented login where different data/options appear
    upon authentication through an application login (the metaphor given
    is Flickr where the public may view photos, authenticated users may
    comment on them, and the owner can edit them).

#### Principles

1.  Any methodology must exhibit graceful degradation. Any
    authentication-blind (herafter referred to as a muggle) client or
    server must be able to interoperate with authentication aware
    servers and clients, though functionality arising from
    authentication is obviously not accessible.
2.  Implementation and selection of the authentication method is a
    matter for consenting clients and servers to agree on, and is out of
    scope for the DAS specification. However, any authentication name
    that exists in the HTTP specification (such as Digest or Basic)
    should conform to that specification. No client or server is
    required to support any particular authentication method.
3.  The implementation should be as lightweight as possible.
4.  The server should advertise available authentication methods

#### Implementation

##### Summary

The base implementation is through use of HTTP headers and extension of
the capabilities specification. Servers and clients should be careful to
adhere to the CORS specifiation where appropriate for cross-origin
request headers.

##### Private Mode

A private server should implement standard HTTP authentication. Clients
are only authorised to access the requested document (i.e. the DAS
response) if they submit valid credentials *and* those credentials
identify a user that is authorised to access the document. In any other
case, a "401 Unauthorized" response is returned to the client. The
client **may** submit credentials in the "Authorization" header and the
server **must** act upon it. DAS servers implementing this method
**must** advertise the capability *authreq/1.0* (or later version). The
capability *authreq* is incompatible with the capability *authopt*.
Either or none should be specified. To specify both is an error.

##### Mixed Mode

For Mixed Mode the server will always return a valid DAS response to a
correctly formed DAS query. In terms of the HTTP specification, this
means that user-agents are always authorised to access the requested
document, regardless of whether they present any user credentials.
However, if credentials are presented, they may provide access to more
data and thus a different document may be returned. Servers using mixed
mode will advertise the availability of authentication through the
capability *authopt/1.0*.

In mixed mode, each DAS response for the server **must** return the HTTP
header *X-DAS-AuthMethods*. The value of this header is an unterminated
semicolon separated list of available methods. Each method comprises the
method name, followed by any parameters required. For example:

` X-DAS-AuthMethods: Basic realm="shareddata"`

When a user is authenticated, the server will return the additional
header *X-DAS-IsAuthenticated: True* (True may be any string containing
at least one non-space character).

In order to authenticate, the client must provide the appropriate
credentials in either or both of the *Authorization* and
*X-DAS-Authorization* headers. If both are set they must contain the
same value. The duplication arises from the inability of some (most)
client libraries to provide access to the *Authorization* header. In
contrast to "private" mode, clients should not expect to receive a 401
response - neither prior to sending authentication details, nor if
authentication details are not valid.

##### Capabilities

The capabilities advertised by a DAS server may change upon
authentication. For example a server may choose to only advertise
writeback as a capability to authenticated clients. However,
authentication capabilities should be expressed at the source level as
the finest level of granularity. A source should have only one
authentication capability (none, authreq or authopt). Mixing public and
private data on the same source is not definable at the current
granularity of capability definitions.

##### Authentication Methods

Authentication is left to the client and server to negotiate. It is
expected that methods using the same names as HTTP methods will be
direct implementations of that method.

##### `das/sources` command

Clients should expect any authentication method for the `/das/sources`
command. For *authopt* servers, the content of the response may change
upon authentication. Servers are not required to publish private mode
sources to unauthenticated clients (and may specifically choose so not
to do).

##### Worked Example/proof of concept

The server at [http://www.compbio.dundee.ac.uk/geneweb/das University of
Dundee](http://www.compbio.dundee.ac.uk/geneweb/das_University_of_Dundee "wikilink")
holds mixed mode data in the data source
[http://www.compbio.dundee.ac.uk/geneweb/das/myseq
myseq](http://www.compbio.dundee.ac.uk/geneweb/das/myseq_myseq "wikilink").
(This is a proof of concept server written in Python on Django. It
implements the sources, features, sequences and authopt capabilities. It
is not a robust production server - please be nice).

A public user can
[http://www.compbio.dundee.ac.uk/geneweb/das/myseq/features?segment=sequence1
retrieve features for the sequence
''sequence1''](http://www.compbio.dundee.ac.uk/geneweb/das/myseq/features?segment=sequence1_retrieve_features_for_the_sequence_''sequence1'' "wikilink").
As this is mixed mode and the muggle web browser does not support mixed
mode (just private and public) the client will only see public data like
this:

&lt;?xml version="1.0" ?&gt; &lt;DASGFF&gt; &lt;GFF
href="http://www.compbio.dundee.ac.uk/geneweb/das/myseq/features?segment=sequence1"
version="1.0"&gt; &lt;SEGMENT id="sequence1" start="1" stop="140"&gt;
&lt;FEATURE id="f1"&gt;&lt;TYPE id="thingy"/&gt;&lt;METHOD
id="invented"/&gt;&lt;START&gt;1&lt;/START&gt;&lt;END&gt;10&lt;/END&gt;&lt;NOTE&gt;This
should be visible to all clients&lt;/NOTE&gt;&lt;/FEATURE&gt;
&lt;/SEGMENT&gt; &lt;/GFF&gt; &lt;/DASGFF&gt;

A non-muggle client will look through the headers and see the following:

x-das-server: Davids Django Dassler x-das-capabilities:
features/1.1,sequence/1.1,authopt/1.0 x-das-version: 1.6
x-das-authmethods: Basic realm='Davids denied DAS demo' x-das-status:
200 ... (other headers snipped)

Our non-muggle client recognises the ''X-DAS-AuthMethods: Basic
realm='Davids denied DAS demo' '' header and can manage Basic
authentication.

It constructs a set of headers (now including the CORS headers which I
will omit for brevity) which include *X-DAS-Authorisation: Basic
RGF2aWQ6ZGl2YUQ=* (Username David, Password divaD) and now gets the
response including the header *X-DAS-IsAuthenticated: True* to indicate
that authorisation was successful.

The response content has changed to include the features that are
visible to this user as well as the public data.

&lt;?xml version="1.0" ?&gt; &lt;DASGFF&gt; &lt;GFF
href="http://www.compbio.dundee.ac.uk/geneweb/das/myseq/features?segment=sequence1"
version="1.0"&gt; &lt;SEGMENT id="sequence1" start="1" stop="140"&gt;
&lt;FEATURE id="f1"&gt;&lt;TYPE id="thingy"/&gt;&lt;METHOD
id="invented"/&gt;&lt;START&gt;1&lt;/START&gt;&lt;END&gt;10&lt;/END&gt;&lt;NOTE&gt;This
should be visible to all clients&lt;/NOTE&gt;&lt;/FEATURE&gt;
&lt;FEATURE id="f3"&gt;&lt;TYPE id="Jim and David\\'s
thingy"/&gt;&lt;METHOD
id="invented"/&gt;&lt;START&gt;30&lt;/START&gt;&lt;END&gt;35&lt;/END&gt;&lt;NOTE&gt;This
should be visible to both Jim and David
only&lt;/NOTE&gt;&lt;/FEATURE&gt; &lt;FEATURE id="f4"&gt;&lt;TYPE
id="David\\'s thingy"/&gt;&lt;METHOD
id="invented"/&gt;&lt;START&gt;40&lt;/START&gt;&lt;END&gt;45&lt;/END&gt;&lt;NOTE&gt;This
should be visible to David only&lt;/NOTE&gt;&lt;/FEATURE&gt; &lt;FEATURE
id="f6"&gt;&lt;TYPE id="David and Tom\\'s thingy"/&gt;&lt;METHOD
id="invented"/&gt;&lt;START&gt;60&lt;/START&gt;&lt;END&gt;65&lt;/END&gt;&lt;NOTE&gt;This
should be visible to David and Tom only&lt;/NOTE&gt;&lt;/FEATURE&gt;
&lt;/SEGMENT&gt; &lt;/GFF&gt; &lt;/DASGFF&gt;

This server has data for three users (David, Tom and Jim with passwords
divaD, moT and miJ). You are welcome to try clients against this server
but do not expect it to be robust.

### Implications

Mixed mode provides a useful layer to allow public visible entry points
to which writeback requests can be made. Muggle clients and servers will
not break and will function as they do now.

### Discussions from the [2010 DAS Workshop](/wiki/DASWorkshop2010 "wikilink") (Legacy content)

As discussed at the [2010 DAS Workshop](/wiki/DASWorkshop2010 "wikilink"),
there are two alternative proposals:

#### Standard HTTP Authentication

In this model, the DAS specification would simply specify that the
existing HTTP authentication strategies
([basic](http://en.wikipedia.org/wiki/Basic_access_authentication) and
[digest](http://en.wikipedia.org/wiki/Digest_access_authentication)) can
be used in DAS, and that clients should be ready to handle these
situations. This strategy is simple to adopt, and has a large number of
existing implementation tools. However, clients and servers must all
maintain independent implementations (with all of the regulatory privacy
concerns) and users have separate login credentials for every DAS
server.

#### Delegated Authentication

In a delegated authentication system, authentication is effectively
"outsourced" to a third party by clients and servers, leaving servers
only to deal with authorisation (i.e. servers maintain lists of users
who are allowed access, but don't have to worry about passwords). The
model is used by systems such as OpenID. However, OpenID itself is
fundamentally tied to user-browser environments due to its use of HTTP
redirects, and therefore cannot be used with DAS (not all DAS clients
are browser based, and even browser-based clients use proxies due to
technical limitations). Therefore, this model is a DAS-specific
implementation of delegated authentication. The below summarises the
steps in an authenticated DAS request:

1.  A DAS client asks the user for his/her "DAS logon" credentials. This
    would be an email address and password. The DAS client is free to do
    this in any mechanism, which will depend on its implementation. For
    example, web-based clients might use HTTPS, desktop clients can use
    a dialog box.
2.  The DAS client forwards the credentials over HTTPS to the DAS
    registry, which is acting as the 'credential provider'. The client
    also provides a list of servers it wishes to access. The location of
    the registry is predefined, as the client implicitly trusts the DAS
    Registry to confirm that the user is who he says he is.
3.  If the password is correct, the DAS registry responds with a token
    for the client to use in requests to each DAS server. These tokens
    allow the client to act on the user's behalf without divulging the
    user's password.
4.  The DAS client initiates a DAS request (e.g. for features) to the
    DAS server using HTTPS, and embeds the user's email address and
    token in the request.
5.  The DAS server forwards the email address, token and its own URL to
    the DAS registry (which it trusts), and asks if the token is valid
    for that user and server.
6.  If the token is valid, and the email address identifies someone who
    is authorised to access the DAS server, the DAS server returns the
    data to the DAS client

Some points about this:

-   Tokens need to be tied to a specific DAS server, as otherwise an
    unscrupulous server could use a token received from a client to
    access data from another server.
-   The registry must check email addresses. Otherwise, a user could
    register an email address they do not own in order to gain access to
    a server.

### Comparison

|                   | HTTP                                                                                                                                        | Delegated                                                                                                                                      |
|-------------------|---------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------|
| Credentials       | The user must maintain usernames and passwords for each and every DAS server.                                                               | The user has a single "DAS password".                                                                                                          |
| DAS client trust  | The user must trust a third party DAS client with his password. Unscrupulous clients can access a user's data from a **single** DAS source. | The user must trust a third party DAS client with his password. Unscrupulous clients can access **all** a user's data from **any** DAS source. |
| Storing passwords | DAS servers, and DAS clients storing passwords between sessions, need to worry about secure password storage.                               | The DAS Registry, and DAS clients storing passwords between sessions, need to worry about secure password storage. Servers do not.             |
| Rescinding access | A user cannot rescind access (for example to a rogue client) without changing all their passwords.                                          | Tokens could be cancellable, enabling the user to visit the DAS registry and rescind access privileges to a client at will.                    |
| Authorisation     | In addition to maintaining user accounts and passwords, DAS servers must also adopt their own mechanism for authorisation.                  | DAS servers must still adopt their own mechanism of authorisation by tying email addresses to internal user lists.                             |

Use of URIs for DAS identifiers
-------------------------------

Often, the use of unique IDs within DAS is poorly executed. Formally
adopting URIs as identifiers (including specifying how URIs are built
from URI references within DAS XML documents) would allow cross
referencing between sequences and annotations within and outside DAS.

Adjacent Feature filter
-----------------------

An optimization to allow efficient implementation of "next/previous
feature" functionality. [Proposed
spec](https://github.com/dasmoth/dalliance/wiki/AdjacentFeatures).

Pagination for DAS
------------------

**March 23,2011**

This is a working document and a proposal for an extension to the [DAS
1.6 specification](/wiki/DAS1.6 "wikilink") in order to provide a mechanism to
paginate the results of a *feature* request similar to the already
included for *entry-points*.

This proposal explains how a specific subset of the response can be
requested.

### New Argument - rows

*rows*, a new argument for the *features* command is added as optional,
so now the request of this command is defined as:

<code>

`SERVER/das/DSN/features?segment=RANGE`  
` [;segment=RANGE]`  
` [;type=TYPE]`  
` [;type=TYPE]`  
` [;category=CATEGORY]`  
` [;category=CATEGORY]`  
` [;feature_id=ID]`  
` [;maxbins=BINS]`  
` [;query=DASQUERY]`  
` `**`[;rows=start-end]`**

</code>

Limits the features returned in the response to those in the given
range, allowing the client to retrieve a smaller cross-section of the
results at any one time. This is particularly important for responses
with large numbers of features (such as an advanced search). The
parameter takes the form start-end.

Note that servers implementing this capability should NOT choose to
limit the rows returned unless the client requests it, and should NOT
choose a different range to that requested by the client. This
capability thus differs the equivalent parameter in the DAS
entry\_points command. Servers that are unable to serve a client's
requested range of rows should respond with an error (see [\#Status
Code](#Status_Code "wikilink") below).

### Response

Two modifications on the features XML response are required to support
this extension:

-   The element **GFF** new optional attribute called *total* reporting
    the total number of features in the whole response(for all the
    segments that excluding the pagination should be returned). The
    addition to the relaxNG should be:

`  `<code>  
`   `<element name="GFF">  
`    `<optional>  
`     `**<attribute name="total"><data type="integer"/></attribute>**  
`     ...`  
`    `</optional>  
`    ...`  
`  `</code>

-   The element **SEGMENT** new optional attribute called *total*
    reporting the total number of features in the specific segment. The
    addition to the relaxNG should be:

`  `<code>  
`   `<element name="SEGMENT">  
`    `<optional>  
`     `**<attribute name="total"><data type="integer"/></attribute>**  
`     ...`  
`    `</optional>  
`    ...`  
`  `</code>

**Note:** The added attributes are defined as optional in the response
for the cases where this extension is not implemented, however their
inclusion is mandatory if this capability is reported.

The server should respond to a features request with a "rows" parameter
by only returning the features in the indicated range. Features in
different segments should be processed sequentially. That is, where
rows=1-5 and the first segment contains 4 features and the second
contains 16, the response would look like this:

<code>

` /das/mysource/features?segment=1:200,800;segment=2:400,1000;rows=1-5`  
` `<GFF total="20">  
`   `<SEGMENT id="1" start="200" stop="800" total="4">  
`     `<FEATURE id="f1">` ... `</FEATURE>  
`     `<FEATURE id="f2">` ... `</FEATURE>  
`     `<FEATURE id="f3">` ... `</FEATURE>  
`     `<FEATURE id="f4">` ... `</FEATURE>  
`   `</SEGMENT>  
`   `<SEGMENT id="2" start="400" stop="1000" total="16">  
`     `<FEATURE id="f5">` ... `</FEATURE>  
`   `</SEGMENT>  
` `</GFF>

</code>

If only the first 4 rows had been requested, the second segment would be
omitted entirely but the total in the GFF element would still be 20. On
the other hand if the requested rows were 6-20, only the 2nd to 16th
features from segment 2 would be returned.

### Capability

A source able to deal with this extension have to reflect this in its
*sources* command by a similar line as:

<capability type="das1:rows-for-feature"   />

### Status Code

If a request is too long that a DAS server is not able to process
it(e.j. Reaching Memory limits) the server should respond with an HTTP
error 500 including a X-DAS-Status header 502, this can be interpreted
to the client as a suggestion to use pagination, or to redefine the
start-stop limits of the rows parameter.
