---
title: DAS/2.1/Spec/Get-Genomic/Types
permalink: wiki/DAS/2.1/Spec/Get-Genomic/Types/
layout: wiki
---

**<big>[DAS/2.1](/wiki/DAS/2.1/Spec/Get-Genomic "wikilink") Types Document
Specification for Genome Data Retrieval</big>**

Overview
--------

Every DAS feature is associated with a particular feature TYPE. DAS
types do not describe a formal type system in that DAS types do not
derive from other DAS types. The DAS type record exists to group
together all features of the same "type" (as defined by the data
provider), link that type to an external ontology term, and describe how
to depict the features.

A DAS annotation server's source document must contain a CAPABILITY
element of type "types". Fetching the corresponding query\_uri returns a
document of content-type "application/x-das-types+xml" listing all of
the types available on the server. There are no query filter parameters
for retrieving the types document.

The following is an example of a DAS annotation server for human,
providing features based on Genscan transcript predictions. Transcript
prediction features for Genscan are specified as type
"<http://www.example.org/das/human/build36/types/genscan_transcript>".
Genscan predictions also include the exons of the transcript, and these
are specified as type
"<http://www.example.org/das/human/build36/types/genscan_exon>".

### Example request and response

Request:

> <http://www.example.org/das2/human/build36/types>

Response:

    Content-Type: application/x-das-types+xml

    &lt;TYPES xmlns="http://biodas.org/documents/das2"
           xml:base="http://www.example.org/das2/human/build36/types/"&gt;
      &lt;TYPE uri="genscan_transcript" 
          title="Genscan transcript predictions"
          doc_href="http://www.example.org/docs/genscan_transcript.html"
          method="GENSCAN 1.0"
          ontology="http://das.biopackages.net/das/ontology/obo/1/ontology/SO/0000673"
          so_accession="SO:0000673"&gt;
      &lt;/TYPE&gt;
      &lt;TYPE uri="genscan_exon" 
          title="Genscan exon predictions"
          doc_href="http://www.example.org/docs/genscan_exon.html"
          method="GENSCAN 1.0" 
          ontology="http://das.biopackages.net/das/ontology/obo/1/ontology/SO/0000147"
          so_accession="SO:0000147"&gt;
      &lt;/TYPE&gt;
    &lt;/TYPES&gt;

Details
-------

A types request returns information about feature types. This includes
the link to an ontology, display information, and a description of the
possible alternative formats. The DAS type record is not part of a type
system in that DAS type records are not derived from other DAS type
records. Instead they link groups of features to an external type
system.

The request for all types on the server is done through the query\_uri
of the "types" CAPABILITY listed in the versioned source entry, as in
the following:

    &lt;CAPABILITY type="types"  query_uri="./types"&gt;

In this case the query\_uri is a relative URL, with the base URL defined
somewhere besides the snippet shown. There must be one and only one
types CAPABILITY for a versioned source.

Fetching that URI returns a document of content-type
"application/x-das-types+xml" containing all of the types.

### Example request and response

Request:

> <http://www.biodas.org/das/sequence/gallus_gallus/March2004/types>

Response:

    Content-Type: application/x-das-types+xml

    &lt;?xml version="1.0" encoding="UTF-8"?&gt;
    &lt;TYPES xmlns="http://biodas.org/documents/das2"
       xml:base="http://www.biodas.org/das/sequence/gallus_gallus/March2004/"&gt;

     &lt;TYPE uri="T1" ontology="http://song.sourceforge.net/SO/five_prime_UTR"
       so_accession="SO:0000204" title="5' UTR"
       description="A region at the 5' end of a mature transcript (preceding 
    the initiation codon) that is not translated into a protein."
       method="N-SCAN 3.0"&gt;
      &lt;PROP key="program_href" value="http://genes.cse.wustl.edu/BrentLab/MB-Lab-Software.html" /&gt;
      &lt;ext:n-scan-options xmlns:ext="http://dalkescientific.com/das-extension"
            informant="human_nscan-10-03-2005.zhmm" sequence="refseqs_hg17"
            parameter="x3j11-Jan-2006.txt" /&gt;
     &lt;/TYPE&gt;

     &lt;TYPE uri="T2" ontology="http://song.sourceforge.net/SO/gene"
       so_accession="SO:0000704" title="strong gene prediction"
       description="Genscan score above 100" method="GENSCAN 1.0"&gt;
      &lt;PROP key="program_href" value="http://genes.mit.edu/GENSCAN.html" /&gt;
     &lt;/TYPE&gt;

     &lt;TYPE uri="T3" ontology="http://song.sourceforge.net/SO/gene"
       so_accession="SO:0000704" title="moderate gene prediction"
       doc_href="http://genes.mit.edu/GENSCANinfo.html"
       description="Genscan score between 50 and 100" method="GENSCAN 1.0"&gt;
      &lt;PROP key="program_href" value="http://genes.mit.edu/GENSCAN.html" /&gt;
     &lt;/TYPE&gt;

     &lt;TYPE uri="T4" ontology="http://song.sourceforge.net/SO/gene"
       so_accession="SO:0000704" title="weak gene prediction"
       doc_href="http://genes.mit.edu/GENSCANinfo.html"
       description="Genscan score between 0 and 50" method="GENSCAN 1.0"&gt;
      &lt;PROP key="program_href" value="http://genes.mit.edu/GENSCAN.html" /&gt;
     &lt;/TYPE&gt;

    &lt;/TYPES&gt;

### TYPES element

The TYPES element has zero or more <a href="#link_element">LINK
elements</a> linking the types document to some other document.

The TYPES element have zero or more TYPE elements. Each TYPE element has
several attributes. The 'uri' attribute is a URI and must be unique for
each TYPE in the document. The URI is used by a feature to identify its
type. Each URI should be individually fetchable, returning a type
document containing the given type record.

The 'ontology' attribute is a URI identifying the formal sequence
ontology term to which the DAS type belongs. Multiple DAS type records
may point to the same ontology term, as for example gene predictions
from different source programs or different representation styles
depending on the score. At present there are no stable ontology URI
schemes so this attribute is optional.

The sequence ontology (SO) is widely used but its identifiers are not
URIs. The 'so\_accession' attribute contains the SO accession number
including the leading "SO:", as in "SO:0000316". Note that the leading
zeros are important. This field should be interpreted as an opaque
string. It is <b>highly recommended</b> that DAS/2 servers feature types
based on the sequence ontology vocabulary.

The optional 'title' attribute is a short title of at most a few words,
meant to be human readable. The optional 'description' attribute
contains up to a paragraph of text (no HTML markup) with more details
about the type record. The optional 'doc\_href' is a URL for a web
browser to display human-readable documentation.

The optional 'method' attribute states where the data came from. It is
at most a few words, meant for people. It may be the name of the
database, analysis program, or person who curated the database. Some
example method fields are:

      curated
      genescan 1.0
      tRNAscan-SE-1.11
      PROSITE 19.24

### Alternative formats

All features are available in a DAS-specific XML format. A data provider
may want to return an alternate format in addition to the base formats.
A simple example is to provide the data in GFF3 format as well as the
standard DAS2 XML. When this occurs the server should add a FORMAT entry
to the "features" capability so clients know which alternate formats are
globally applicable.

Some of the alternative formats are data type specific. For example, a
server may provide the ABI trace data for each sequencing read. The data
provider may model this as a single feature covering the entire genome
where the feature type record links to a new ontology term
"<http://example.com/das2/feature/sequencing_read>". It also declares
that the data is available in "abi" format. If the client recognizes the
ontology type and format name it can retrieve the trace data for a
region of interest by doing a feature query with the appropriate
<a href="#feature_filters">feature filters</a> and adding "&format=abi"
to the query string.

A server indicates support for an alternate format using a FORMAT
element. A TYPE element has zero or more FORMAT elements, each with a
'name' attribute. These list the supported formats for features of the
given type. The reserved format names are "das2xml", "count" and "uris".
See the FEATURE (detailed) section for more details.

### Extending the type record

Some type records may need to provide additional information, for
instance more details about how the features were generated. There are
two ways to extend the basic type record.

#### PROP element

The first is through a property table. The table is a list of zero or
more PROP elements. Each PROP element has a 'key' and 'value' attribute,
mapping the given key to value. Duplicate keys are allowed. There are no
pre-defined key names though de facto ones will likely arise through
use. The order of the PROP elements is arbitrary and must not affect the
meaning of the keys or values.

For more complex extensibility use non-DAS XML elements. Any number of
XML elements may occur after the property table so long as the outermost
elements are not in the DAS namespace. Clients must ignore unknown
elements.
