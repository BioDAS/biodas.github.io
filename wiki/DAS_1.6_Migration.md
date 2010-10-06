---
title: DAS 1.6 Migration
permalink: wiki/DAS_1.6_Migration/
layout: wiki
---

This page contains information to facilitate the migration of DAS
clients and servers to version 1.6 of the specification.

Most of the changes between versions 1.53, 1.53E and 1.6 are additional
and non-destuctive. As a result, many servers and clients implementing
core parts of the specification in different versions will continue to
work together without issue. There are exceptions however, as detailed
below. If you are at all unsure as to the version you should use, ask on
the [mailing list](/wiki/BioDAS:Community_Portal "wikilink").

Creating clients
----------------

You should engineer your client to work with DAS 1.6. However, you
should be aware that many DAS servers do not and will not update, and
therefore may wish to be fault tolerant and consider including a "legacy
mode" to deal with, in particular:

-   servers which do not implement a *sources* command, but a *dsn*
    command instead
-   sources which provide groups in the features command instead of
    parents/parts

Creating servers
----------------

If you are creating a DAS server, it therefore makes sense to use the
latest version you can given the capabilities of the client(s) you
expect it to be used with. The server will be more future proof, and
have more features and fixes. Whether this means using 1.6 depends on
the client.

Clients are generally engineered to work with servers of equivalent or
previous version (e.g. a 1.6 client will usually support 1.53 or 1.53E
servers). Functionality may not be complete however, simply because some
things possible in 1.6 are not possible in 1.53 (for example). In
addition, most 1.6 servers are still likely to work fine even if the
client does not support version 1.6. This is especially true if the
client supports 1.53E. The reason for this is that most of the
specification changes are minor and non-destructive (e.g. removal of
attributes rarely used, addition of new optional attributes). The major
difference to be aware of which may prevent a 1.6 server from working in
a legacy client is if the DAS source offers hierarchical features (e.g.
genes, transcripts, exons). DAS 1.6 handles this in a different (more
flexible) way, and so the features may not be drawn correctly. It should
be noted that most DAS sources do not have hierarchical features and
will therefore not be affected by this change.

For more specific information and advice, it is recommended you ask on
the [mailing list](/wiki/BioDAS:Community_Portal "wikilink").

Client Version Support
----------------------

Below is a table showing the versions of DAS supported by existing
client software. If you can add to or update this information, please do
so.

|           |                    |                           |
|-----------|--------------------|---------------------------|
| Client    | Versions supported | 1.6 migration target date |
| Ensembl   | 1.53, 1.53E        | 2011                      |
| Dalliance | 1.53,1.53E,1.6     | complete                  |
| Dasty3    | 1.53,1.53E,1.6     | complete                  |

Server Version Support
----------------------

To find the version of the specification supported by a server, you
should consult the [DAS Registry](/wiki/DasRegistry "wikilink").
