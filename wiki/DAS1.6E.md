---
title: DAS1.6E
permalink: wiki/DAS1.6E/
layout: wiki
---

Proposals for DAS 1.6 Extensions to be added after the 1.6 spec has been
officially released.

DAS writeback
=============

**June 10,2009**

This is a working document and a proposal for an extension to the [DAS
specification](http://biodas.org/documents/spec.html) in order to
support writeback capabilities. A writeback server should have, at
least, the methods for the basic reading/writing operations, which in
Databases is recognized as CRUD(Create, Read, Update and Delete). The
reading component is already solved for the DAS protocol, even is
possible to affirm that all the current commands in the specification
are reading commands for different kind of information.

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
elements of this format are explained in the [DAS
specification](http://biodas.org/documents/spec.html) An example of the
Document is below, and as it will be explained later the same format
could be used for the input or the output of one of the HTTP methods.
Extra information can be required depending of the implementation as
metadata of the operation. For those cases the element NOTE should be
used in a notation KEY=VALUE. In the example below this notation is used
to represent the OpenId of the user who sent or is sending this feature
to the server. <code>

<?xml version="1.0" standalone='no'?>
<!DOCTYPE DASGFF SYSTEM "http://www.biodas.org/dtd/dasgff.dtd">
<DASGFF>  
` `<GFF version="1.0" href="http://www.ebi.ac.uk/das-srv/uniprot/das/uniprot/features?segment=P05067">  
`   `<SEGMENT id="P05067" start="1" stop="770" version="7dd43312cd29a262acdc0517230bc5ca">  
`      `<FEATURE id="UNIPROTKB_P05067_KEYWORD_Disease" label="Disease mutation">  
`       `<TYPE id="BS:01019" category="inferred by curator (ECO:0000001)">`disease`</TYPE>  
`       `<METHOD id="UniProt">`UniProt`</METHOD>  
`       `<START>`10`</START>  
`       `<END>`40`</END>  
`       `<SCORE>`0.0`</SCORE>  
`       `<ORIENTATION>`0`</ORIENTATION>  
`       `<PHASE>`-`</PHASE>  
`       `<LINK href="http://www.uniprot.org/uniprot/P05067">[`http://www.uniprot.org/uniprot/P05067`](http://www.uniprot.org/uniprot/P05067)</LINK>  
`       `

<NOTE>
Adding a new feature!

</NOTE>
<NOTE>
USER=<http://user.myopenid.com>

</NOTE>
`     `</FEATURE>  
`   `</SEGMENT>  
` `</GFF>  
</DASGFF>

</code>

### HTTP METHODS (DAS writeback methods)

#### POST

The HTTP method *POST* should be used to create a feature in the server.
The writeback document should be sent to the server in a POST variable
called ''' *content* '''. The server should create an identifier for
this feature. This id should be the URI formed by the concatenation of
the URL of the writeback server and some unique number for the server(a
consecutive number). The response should be based in the HTTP headers of
the DAS specification. (i.e. 200 OK, 500 Server error, etc.). The
content of the response should be a GFFDAS format with the information
that was created in the database therefore if everything is correct the
response will be almost the same that the input document. The only
difference will be the metainformation added for the server as notes,
for example

<code>

<NOTE>
USER=<http://user.myopenid.com>

</NOTE>
<NOTE>
VERSION=1

</NOTE>
<NOTE>
DATE=2009-06-10 18:11:30.672644

</NOTE>
</code>

#### PUT

The behavior of the method *PUT* is very similar to the described for
*POST*. However in this case the writeback document is the content itsel
of the request(i.e. Is not embedded in any parameter). An important
difference is the management of the id, which is defined as the value in
the field *id* of the element *FEATURE* if this value is a valid URI,
otherwise is the URI formed by the base URL in the field *href*
concatenated with the value in the field *id* of the element *FEATURE*.
For example for the same writeback document of above the id in the
element ''FEATURE will be: <code>

<FEATURE id="http://www.ebi.ac.uk/das-srv/uniprot/das/uniprot/features/UNIPROTKB_P05067_KEYWORD_Disease" label="Disease mutation">

</code> The response should follow the same rules as the *POST* method.

#### DELETE

The *DELETE* method doesn't require a writeback document for the input,
because in order to DELETE a feature is just necessary to identify it.
The identification of a feature will be reconstructed by its id and the
id of the segment where it will be deleted. The openid is also require
for this transaction. The format of the HTTP connection should be:
<code>

`DELETE [server]?featureid=[featureid]&user=[openid]&segmentid=[segmentid]`

</code>

A successful response SHOULD be 200 (OK) if the response includes an
entity describing the status, 202 (Accepted) if the action has not yet
been enacted, or 204 (No Content) if the action has been enacted but the
response does not include an entity. The content of the response should
be in the same GFF format but the parameter *label* of the element
*FEATURE* should be **DELETED** as in: <code>

<FEATURE id="http://writeback/92" label="DELETED">

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
<!DOCTYPE DASGFF SYSTEM "http://www.biodas.org/dtd/dasgff.dtd">
<DASGFF>  
` `<GFF version="1.0" href="http://localhost:8080/MyDas/das/writeback/historical?feature=http://writeback/9">  
`   `<SEGMENT id="P05067" start="1" stop="770" version="7dd43312cd29a262acdc0517230bc5ca">  
`     `<FEATURE id="http://writeback/9" label="Disease mutation">  
`       `<TYPE id="BS:01019" category="inferred by curator (ECO:0000001)">`disease`</TYPE>  
`       `<METHOD id="1">`UniProt`</METHOD>  
`       `<START>`143`</START>  
`       `<END>`189`</END>  
`       `<SCORE>`0.0`</SCORE>  
`       `<ORIENTATION>`0`</ORIENTATION>  
`       `<PHASE>`-`</PHASE>  
`       `

<NOTE>
testing note

</NOTE>
<NOTE>
USER=<http://user.myopenid.com>

</NOTE>
<NOTE>
VERSION=1

</NOTE>
<NOTE>
DATE=2009-05-25 14:22:39.705735

</NOTE>
`       `<LINK href="http://www.uniprot.org/uniprot/P05067">[`http://www.uniprot.org/uniprot/P05067`](http://www.uniprot.org/uniprot/P05067)</LINK>  
`     `</FEATURE>  
`     `<FEATURE id="http://writeback/9" label="DELETED">  
`       `<TYPE id="" category="" />  
`       `<METHOD />  
`       `<START>`0`</START>  
`       `<END>`0`</END>  
`       `<SCORE>`0.0`</SCORE>  
`       `<ORIENTATION>`0`</ORIENTATION>  
`       `<PHASE>`-`</PHASE>  
`       `

<NOTE>
USER=<http://user.myopenid.com>

</NOTE>
<NOTE>
VERSION=2

</NOTE>
<NOTE>
DATE=2009-06-10 17:58:11.83588

</NOTE>
`     `</FEATURE>  
`   `</SEGMENT>  
` `</GFF>  
</DASGFF>

</code>

### User Authentication

The control of who is authorized or not to made modification in the
writeback is an source implementation issue, however the authentication
of the user should be done using [OpenId](http://www.openid.org/) as
seen in the examples above the openid user should be sent through the
NOTE element: <code>

<NOTE>
USER=<http://user.myopenid.com>

</NOTE>
</code>

DAS search
==========

Support for alternative content formats
=======================================

Use of URIs for DAS identifiers
===============================
