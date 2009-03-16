---
title: DASworkshop200903Day3
permalink: wiki/DASworkshop200903Day3/
layout: wiki
---

Doodle Poll: [poll](http://doodle.com/68bxciw5vaqq7icw)

This page is for people attending day 3 of the workshop to put down
their ideas and volunteer to host a session on a topic or write some
minutes of the meeting on the biodas wiki site, so we have a record of
what was discussed and what the outcomes were or ideally some code to
work with.

-   My current suggestion for the format would be to have the 1.6 spec
    and the DAS1/DAS2 merger topics discussed in the first 2 hrs of the
    day and be open to everybody. Then have groups split up into the
    remainder of the topics?

### DAS 1.6 spec and where to go with the Registry

Host: Jonathan Warren and Andy Jenkinson Secretary: Jonathan Warren and
Andy Jenkinson

[1.6\_Examples](1.6_Examples "wikilink")

DAS1.6 spec discussions:

-   Generally 1.6 was accepted as a good idea ,Minor changes to 1.6
    document:
-   Document XDAS server version so its implemented correctly.
-   Alignment clarified in the das spec?
-   BOSC proposed as a release date for the 1.6 spec
-   The entry\_points cmd is proposed as being compulsory in 1.6 for
    both annotation and reference servers?

`this caused a lot of discussion as to how to handle cases where there are 1000's of entry_points. A consensus seemed to be that the spec should allow people`  
`to specify a range eg. 1-1000, then 1000-2000 in the request in a similar way that you can for the alignments you want returned in the alignment 1.53E spec.`  
`There should be a DAS standard default range always returned if no params sent say 1-10.`  
`The response should always return a count for the total number of entry_points available for the source.`  
`We need to provide a explanation as to why the above is available.`  
`   `

-   IUPAC names for moltype was suggested e.g. deoxyribonucleic
    acids (DNA).
-   it was suggested that the registry should enforce the number of
    types available in a source.
-   we need to explain what cvId stands for in the spec (Controlled
    Vocabulary Identifier) and why people should be encouraged to
    use it.
-   JS suggested that the maxbins should encompass a returned value that
    says how the data is being made i.e. whether it is an average or an
    extreme of the data in those bins.

`this at least should be some kind of free text field.`  
`response should also say if returning 200 of 10,000??`

### DAS1/DAS2 merger

### DAS writeback

### Adapting Java DAS client libraries for alignment and PDB file retrieval for clients that dont use BioJava

Not discussed.

### Javascript das client/server library (allow DAS server discovery and retrieval on the browser page)

Meeting held on Thursday as relevant people were available after
workshop:

MINUTES Attendees: Bernat, Greg, Andy, Jonathan, Gustavo and Rafael

On Thursday 12 we talked about JavaScript DAS client libraries. We
identified three different ways to create user interfaces using
javascript.

-   1.- Clients based on the google maps idea. Creating images on the
    server and using javascript on the client to manage tailing images.
    Victor de la Torre show a nice example on his presentation
    "collaborative annotation tool (MaDAS)"

<!-- -->

-   2.- Clients drawing graphics using canvas. Canvas is a new HTML
    element which can be used to draw graphics using scripting
    (usually JavaScript). Following this idea the client collects the
    information to render it on the canvas. Bernat Gel gave some
    examples with his genome viewer "DASgenexp".

<!-- -->

-   3.- Client creating graphics using HTML elements and transparent
    shaped images. In this case the client creates graphic
    representations creating HTML elements through the DOM
    and javascript. We can find an example in Dasty2 and the
    Karyotype Viewer.

To be able to share javascript on web DAS clients rendering graphics on
the client side we proposed to create a standard javascript library. On
a web client we can identify three general stages: Request, Parsing and
Visualization. Since visualization it is more up to the developer and
the necessities of the project we proposed to create this standard
library for requests and parsing. Rafael agreed to create the first
prototype for this library.

### DAS interfaces to sequence analysis tools

Not discussed as a group due to time constraints

### BioSapiens ontology lookup and general Ontologies

How to represent the ontology in DAS, e.g. JR Macias' "term" command

-   <http://biocomp.cnb.csic.es/VisualOmics>

#### DAS vs. ontologies, how and why

This topic was suggeseted because I'd like to understand how
'ontologies' (in general) are implemented in DAS and how this relates to
the feature annotation models in [Chado](/wiki/Chado "wikilink") or
[GFF3](/wiki/GFF3 "wikilink"), for example. How should an annotator use an
ontology? How should the use of an ontology be reflected in the client
behaviour? What about DAS ontology servers (and the 'term' DAS command)?
This topic dosn't really fit with the idea of a 'hackathon', but is a
very interesting aspect of DAS. i.e.

What are:

-   [Ontodas](/wiki/Ontodas "wikilink")
-   [DAS Protein Ontology](/wiki/DAS_Protein_Ontology "wikilink")
-   [Ontology Lookup Service](/wiki/Ontology_Lookup_Service "wikilink")
-   The [BioSapians](/wiki/BioSapians "wikilink") (feature type?) ontology
-   The ontology of 'input filters' from [EMBRACE](/wiki/EMBRACE "wikilink")
    This is an effort by the EMBRACE project to define ontologies for
    use in registering web services in the EMBRACE registry and later in
    the BioCatalogue registry. The ontology will cover the terms used in
    EMBRACE servcies, including BioMart and EMBOSS - both of which
    already have well-defined terms which will be used to populate
    the ontology. This will be contributed to the OBO foundry (like the
    other ontologies used in DAS) and can be extended to cover
    algorithms to give a better description of a service, its inputs,
    parameters and outputs. Contact Peter Rice pmr@ebi.ac.uk for
    more information.

  
???

**Discussion group notes**

-   similar to attribute, but includes hierachical structure

<!-- -->

-   if treated like coordsys, but additional layers hidden

<!-- -->

-   use case: anatomical ontology, uses "term" as query command;

but more general terms return too much data, response needs to be cut

-&gt; proposed solution: use "maxbins"-like functionality

-&gt; For DAS 1.6: adopt ontology from 1.53E as cvID and method

a) Sequence Feature Ontology (SFO)

add terms if necessary, other ontology may need to be added for
anatomical eg.

b) evidence codes

example:
<TYPE id="exon SO:0000147" category="inferred from RT-PCR experiment (ECO:0000109)">exon</TYPE>

-   Add to Registry: Cache Ontology, add links to master ontology
    files (PSI?) and ontology browser (EBI?)

### Searching, Filters and tags
