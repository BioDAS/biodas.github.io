---
title: DAS/2.1/Spec/Writeback
permalink: wiki/DAS/2.1/Spec/Writeback/
layout: wiki
---

**<big>[DAS/2.1](/wiki/DAS/2.1/Spec "wikilink") Writeback
Specification</big>**

DAS/2.1 Writeback Specification

DAS 2.1 annotation servers may support the creation, editing, and
deletion of DAS resources as requested from DAS clients. This is also
referred to as DAS writeback. A DAS2 resource is any entity represented
in DAS2 XML that has a URI, and thus a "uri" attribute. The writeback
URI is specified in the versioned source record of a DAS sources
document using a CAPABILITY element of type "writeback", for example:

<CAPABILITY type="writeback" query_uri="http://www.biodas.org/das2/h_sapiens/v35/writeback" />

At most one writeback CAPABILITY element is allowed per versioned source
(???).

The client sends an HTTP POST to the URI of the writeback capability,
which is found by resolving the capability's query\_uri to a URI. The
content of the writeback POST is XML specifying what DAS resources are
being created/edited/deleted. Writeback uses the same XML schemas as
returned for DAS2 features, types, and segments as specifiied in the
DAS2 retrieval specs, except for two additions described below. The
servers response to a successful writeback is a modified version of the
POSTed content. Use of formats other than XML for representing the DAS
resources is currently not supported.

\[For creation/editing/deletion of DAS2 sources and versioned sources
there also needs to be a pointer somewhere to the writeback URI for a
server's sources doc\].

Writeback Request
-----------------

For DAS 2.1 sources, segments, types, and features, the content-type of
the XML returned is the same as in DAS 2.1 retrieval:

`   sources --> application/x-das-sources+xml`  
`   segments --> application/x-das-segments+xml`  
`   types --> application/x-das-types+xml`  
`   features --> application/x-das-features+xml`

To include a comment use the <PROP> element of the root DAS2 element

New Resource URIs
-----------------

A special syntax is needed to guarantee that a new request for DAS2
resource creation won't be mistaken for editing of a previous resource
due to a client accidentally using the same URI as an existing feature
for the new feature creation request (this is a problem only in the
POST, afterward not an issue since server is in charge of permanent URI
assignment). These client-assigned URIs have the syntax "das-private:"
followed by an alphanumeric string of at least 1 and no more than 20
characters. The legal characters are 0-9, A-Z and a-z. Some examples
are: "das-private:0", "das-private:AAAA" and
"das-private:ThisIsPrivate007".

Deletes
-------

<DELETE uri="xyz" />

Writeback Response
------------------

The server is free to make the requested modifications, as well as other
modifications as it sees fit. The server responds to a request by
sending back an XML representation of the modifications. In the simple
case where the server makes no modifications of its own, this is just
the client's POST XML content mirrored back.

In addition to a required "uri" attribute, each DAS2 resource has an
optional "old\_uri" attribute reserved for use in writeback. A server
may change the resouce's URI during writeback. If this happens then in
the server's response the "old\_uri" attibute contains the URI reference
sent to the server and the normal "uri" attribute contains the new URI
reference determined by the server.

For every DELETE completed, the server should mirror the DELETE back.
However a server \_may\_ choose not to actually do a delete -- for
example servers that maintain a complete versioning history. If this is
the case then the XML element for the entity that wasn't deleted should
be returned as if it was an edit request, so a server can modify the
entity if it chooses to (for example, changing the URI to indicate a
"deleted" entity without actual removal from the server?)

Transactional Integrity
-----------------------

A DAS2 writeback server should support transactional integrity (actions
should be atomic, isolated, and recoverable -- see for example
[here](http://safari.oreilly.com/0321375777/ch13) for more details).
Each HTTP POST should be considered a unit of transactional integrity.
So for example if there are multiple edited features in the POST
content, the server should make the modifications necessary to edit all
of the features and return an HTTP OK status, or else edit none of them
and return an HTTP error status.

Error Handling
--------------

Concurrent Modifications
------------------------

If a problem, use locking. Otherwise relies on database -- it's up to
database whether a curation can be "branched" -- should be easy to do if
database uses unique URIs for each version of a curation. Then branching
is just annotation A being edited once to yield annotation B1 and a
second time to yield B2 -- both are valid. If a database doesn't want to
support this, it's easy enough to throw an error (resource no longer
available?) if someone attempts to edit A after B1 has already been
created.

Handling Feature "Splits"
-------------------------

Splitting feature A into B and C  
<DELETE uri="A" />  
&lt;FEATURE uri="B" ...  
&lt;FEATURE uri="C" ...  

Handling Feature "Merges"
-------------------------

Merging B and C into A  
<DELETE uri="B" />  
<DELETE uri="C" />  
&lt;FEATURE uri="A" ...  

