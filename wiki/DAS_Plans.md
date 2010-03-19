---
title: DAS Plans
permalink: wiki/DAS_Plans/
layout: wiki
---

Future ideas for improvements to the DAS spec:

### Hierarchical features

Features should have parents and children, this could be accomplished
with additional tags. This can be seen as either replacement or
complementary to groups. For example, groups could be seen as more
generic logical and visual groupings (e.g. data from the same
sample/tissue). Implementation of a proper hierarchy will enable support
for assembly traversal (which isn't used at present and is intended to
be removed).

Update: this functionality is now incorporated into the [DAS
1.6](/wiki/DAS1.6 "wikilink") specification.

### Writeback

Read/write DAS sources. This is currently being worked on. The
specification is based on the DAS/2 writeback portion.

Update: this functionality is now described as a [DAS 1.6
extension](/wiki/DAS1.6E "wikilink").

### Annotating features

A possible tie-in with writeback, this could allow:

1.  users to add, edit, delete, tag or comment on features
2.  servers to postprocess or highlight other servers' features

### Migrate 1.53E extensions

After some minor updates to ensure consistency with the rest of the
spec, it is intended that extensions such as alignment be incorporated
into the core spec.

Update: this work is now incorporated into the [DAS
1.6](/wiki/DAS1.6 "wikilink") specification.

### Improvements for dense features

Especially for DAS sources with simple per-base annotations, speed is an
issue. The *maxbins* extension goes a long way to helping, but there are
other options:

1.  move type etc outside each feature to avoid replication
2.  allow a single feature to contain a set of scores across a region of
    sequence
3.  alternative content negotiation

### Bridging coordinate systems

Alignments provide a way to bridge across 1:1 coordinate systems (e.g.
UniProt protein sequence &lt;--&gt; Ensembl protein sequence). We can
formalise this, and extend to cover genomic-protein bridging (i.e. 1:3
relationships).
