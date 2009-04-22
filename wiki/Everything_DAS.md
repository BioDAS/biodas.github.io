---
title: Everything DAS
permalink: wiki/Everything_DAS/
layout: wiki
---

*Last Updated 21 April 2009*

The intention of this document is to bring together and add to all the
documentation available on the WWW for the DAS system. The content on
these pages draws from many sources of information and thus has many
contributors. Eventually the intention if that this document will be a
set of instructions that you can print out and use as reference
documentation or a good read. If you find any errors on these pages and
pages that it links to then please contact me ([Jonathan
Warren](mailto:jw12@sanger.ac.uk)) to let me know. Any suggestions and
contributions are also welcomed. As this is now in wiki form you can log
in and edit/add things yourself.

What is DAS?
------------

As biological databases are becoming so large with the advent of high
throughput technologies such as sequencers and microarray chips it is
becoming increasing difficult to download all the data relevant for a
research team. DAS gets around this by keeping the data stored with it's
originators and allows users around the world to access just the
relevant parts they need at any one time. Put another way: by making use
of DAS you can take advantage of being able to view integrated
information from multiple sources, without these sources needing to be
aware of each other. You can also add your own DAS data source, perhaps
privately in your own institution and then view the information served
from this source in the context of features from other institutions. DAS
stands for Distributed Annotation System. It was originally set up to be
used with genomic information where annotations/features are layered on
top of a reference sequence , usually a genome. The idea is that a
genome browser such as ensembl or GBrowse (both DAS clients in this
scenario) can be used to look at annotations from data sources both that
exist on the same server/machine the browser is running on and display
annotations in the same view from data sources (data served by DAS
servers) that could be on the other side of the world (communicating via
the WWW). The DAS system consists of the DAS Registry
www.dasregistry.org as well as DAS Servers and Clients. The Registry is
there to enable people and computers to easily find the DAS data sources
available around the world and also to help these data sources conform
to the specifications. It's important that data served by DAS servers
conform to enable the interoperability of different clients and servers
around the world.[1.0](http://www.biodas.org/documents/spec.html) is
currently supported and work is starting on supporting the new [The 1.6
spec](http://www.biodas.org/wiki/DAS1.6) that is the latest and soon to
be official DAS spec that mainly focuses on genomic annotations but also
refers to the *E*xtentions specified in the 1.53*E* spec below. [The
1.53E spec](http://www.dasregistry.org/spec_1.53E.jsp) contains up to
date specifications for servers and clients that support information
that can be exchanged using DAS that is not genome centric. Types of
data include Proteins- Structures and alignments, Molecular
Interactions, volume map data.

### Current Status/ DAS specifications 1.5, 1.53E, 1.6, 2.0 and Future Intentions

Currently DAS 1.5 is the most widely used and supported together with
1.53E. DAS 2.0 is quite different and is really running in parallel to
the other 2 versions of DAS. After the 2009 workshop it was generally
agreed that most of the useful additional features that 2.0 provides is
now or very soon to be implemented in DAS 1.6E and it's subsequent
incarnations and thus DAS2.0 is now considered redundant. If you wish
your data to be widely accessible then use the [The 1.6
spec](http://www.biodas.org/wiki/DAS1.6) and [The 1.53E
spec](http://www.dasregistry.org/spec_1.53E.jsp) documents as your
guide.

Setting up a DAS Server
-----------------------

There are several different options available for setting up a DAS
server. All are either written in PERL or Java.

### Servers available

| Name      | Programming Language | advantages                                                                             | disadvantages                                                                                      |
|-----------|----------------------|----------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------|
| Dazzle    | Java                 | Standard implementation, includes support for extensions (structure, interaction, vol) | Some people say it can be hard to configure and deploy if you are not used to Java web development |
| Proserver | PERL                 | Standard implementation includes support for extensions (structure, interaction, vol)  |                                                                                                    |
| MyDAS     | Java                 | Some people say it's easier to set up and configure than Dazzle                        | Doesn't support extensions currently                                                               |
| LDAS      | PERL                 | Very Easy to set up?                                                                   | Limited support for DAS functionality and sources                                                  |

### Dazzle

Dazzle is currently the standard/default implementation for Java users-
however MyDas (mentioned below) is popular.

#### Dazzle Eclipse Tutorial

[Dazzle\_Tutorial](/wiki/Dazzle_Tutorial "wikilink") This tutorial takes you
through setting up Dazzle in eclipse and then shows you how to add your
own plugins

#### Getting Dazzle

<http://biojava.org/wiki/Dazzle#Getting_Dazzle> The latest version from
the cutting edge source code is available here from
subversion:[<http://www.derkholm.net/svn/repos/dazzle/>](http://www.derkholm.net/svn/repos/dazzle/)

#### Using ready made plugins for datasources

<http://biojava.org/wiki/Dazzle:plugins> <font color="red">More examples
needed here and tips for using mysql etc?</font>
<http://biojava.org/wiki/Dazzle:deployment>

#### Writing your own plugin

<http://biojava.org/wiki/Dazzle:writeplugin>  
[How to write a plugin using
eclipse](http://biojava.org/wiki/Dazzle:eclipse) <font color="red">more
on what interfaces need to be implemented and give a full example that
implements all needed functionality such as sources.cmd and coordinate
system etc.</font>

#### Deploying an Ensembl Reference Server

[link to Ensembl reference server
instructions](http://biojava.org/wiki/Dazzle:Ensembl)

### MyDas

[Information about MyDas can be found
here.](http://code.google.com/p/mydas/)

### Proserver

[Proserver Page at the Sanger
Institute.](http://www.sanger.ac.uk/Software/analysis/proserver/)  
[Proserver
Tutorial](http://proserver.svn.sourceforge.net/viewvc/proserver/trunk/doc/proserver_tutorial.html)  
[Guide to
Proserver](http://proserver.svn.sourceforge.net/viewvc/proserver/trunk/doc/proserver_guide.html)  

Implementing the latest specs
-----------------------------

One of the most important additions to the recent specifications is the
sources cmd. This gives essential information such as who maintains the
das source and what coordinate systems it uses which is essential
information for a large distributed system like DAS and is needed by the
registry and clients in order to use the source correctly. Dazzle just
uses a sources.xml document to serve this information that has to be
written by the DAS server owner, but proserver will create a sources
document for you if you specify the extra information needed in the
initialisation file (proserver.ini).

Proserver example of config to implement sources cmd:

`coordinates = TAIR_8,Chromosome,Arabidopsis thaliana -> 1:2000,3000`  
`properties  = key1 -> value1 ; key2 -> value2`  
`mapmaster   =`  
[`http://www.gramene.org/das/Arabidopsis_thaliana.TAIR8.reference`](http://www.gramene.org/das/Arabidopsis_thaliana.TAIR8.reference)  
`capabilities = features -> 1.0`

The coordinates data is taken from the
coordinates/registry\_coordinates.xml file, which is an archived copy of
the list of coordinates available in the DAS registry. Specifying the
name (or URI, actually) and test range is enough, ProServer will pick up
the rest from the XML file. If the full data is not picked up, you may
need to update the coordinates XML file from the registry
(http://www.dasregistry.org/das/coordinatesystem). If your coordinate
system is not in the Registry, an admin can add it for you.

#### Protein Annotations and Ontologies

For an explanation of ontologies usage within the DAS protocal look here
[ontologies](http://www.dasregistry.org/extension_ontology.jsp)

### Testing your implementation

### Validation and Registering of your Server

#### RelaxNG and other validation in the Registry

The DAS Registry uses [RelaxNG](http://relaxng.org/) to validate the xml
responses from DAS servers before allowing them to register as a valid
das source. RelaxNG is essentially a document like a dtd except that it
uses an xml syntax that is easy to learn quickly. The registry uses the
documents found at the following
<http://www.dasregistry.org/validation/> and has one document for each
of the DAS commands (note you may need to right click "view the source"
to see anything on these pages in a web browser)
[features.rng](http://www.dasregistry.org/validation/features.rng),
[sources.rng](http://www.dasregistry.org/validation/sources.rng),
[alignments.rng](http://www.dasregistry.org/validation/alignments.rng),
[structure.rng](http://www.dasregistry.org/validation/structure.rng),
[entry\_points.rng](http://www.dasregistry.org/validation/entry_points.rng),
[interaction.rng](http://www.dasregistry.org/validation/interaction.rng),
[sequence.rng](http://www.dasregistry.org/validation/sequence.rng) and
[types.rng](http://www.dasregistry.org/validation/types.rng).

The DAS Registry
----------------

### Introduction to the DAS Registry

The DAS registry can be found at <http://www.dasregistry.org> and serves
as a central place for discovering DAS sources from around the world and
for validating the sources. There is a user interface for interrogating
the sources and ways for clients to also interrogate the sources.
Support for searching sources based on Ontologies is likely to be
included in future releases. The number of sources registered is set to
increase rapidly to accommodate the ensembl genomes project data and the
general increase in numbers of sequenced genomes. The registry will thus
have to be modified in order to cope with this increase in data. The
user interface has a warning sign next to any of the sources that have
not been valid for two days or more (if this is the case, the registry
will have sent an email to the administrator for the data source
informing them of this fact).

### Connecting to the Registry Programmatically

There are several commands that can be used to query the registry
including: The sources cmd with optional parameters: label, organism,
authority, capability, type and unique source\_id. You can also use the
organsim, coordinatesystem and lastmodified commands. For examples see
[Scripting](http://www.dasregistry.org/help_scripting.jsp) an example of
a java classe written using Dasobert to access the Registry is here
<http://www.derkholm.net/svn/repos/dasobert/trunk/doc/examples/ContactRegistry.java>

### Adding a large set of data sources to the registry

The registry can automatically load a large set of data sources from the
sources.xml that is returned from the sources cmd. If you wish to load a
large set of sources you can contact dasregistry@sanger.ac.uk and ask
for your data sources to be loaded. Please note that your sources must
have valid coordinate systems that are in the registry and a valid
sources document. You can do an initial test at
<http://www.dasregistry.org/validateServer.jsp> and select the sources
capability for your server.

Discovering DAS sources programmatically
----------------------------------------

The registry produces it's own sources.xml in response to the url
request <http://www.dasregistry.org/das1/sources> and this can be used
by clients to get information on the many DAS sources available around
the world and what there capabililties are. Currently there is no way of
clients finding out if the registry knows if the sources are valid or
not.

Setting up a DAS client
-----------------------

### Currently Available DAS Clients - table?

| Name    | Description                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Programming Language | Links                                               |
|---------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------|-----------------------------------------------------|
| GBrowse | quote from GBrowse Website "GBrowse\[1\] is the most popular viewer in GMOD. For a list of GBrowse and GMOD installations see the GMOD Users page. For a demo of its features, try the [WormBase](http://www.wormbase.org/db/seq/gbrowse/wormbase/), [FlyBase](http://flybase.org/cgi-bin/gbrowse/dmel), or [Human Genome Segmental Duplication Database](http://projects.tcag.ca/cgi-bin/duplication/dupbrowse/human_b35) web sites. Spec DAS 1.53E and 1.6 soon | PERL                 | <http://gmod.org/wiki/Gbrowse>                      |
| EnsEMBL | EnsEMBL is a web based genome browser and database system which supports DAS 1.53E and soon 1.6                                                                                                                                                                                                                                                                                                                                                                   | PERL                 | [<http://www.ensembl.org/>](http://www.ensembl.org) |
| IGB     | is an application built upon the GenoViz SDK and Genometry for visualization and exploration of genomes and corresponding annotations from multiple data sources                                                                                                                                                                                                                                                                                                  | Java                 | <http://genoviz.sourceforge.net/>                   |
| Jalview | A multiple sequence alignment editor & viewer                                                                                                                                                                                                                                                                                                                                                                                                                     | Java                 | <http://www.jalview.org/>                           |
| Dasty2  | Dasty, a protein DAS client is implemented for visualising protein sequence feature information. The client is able to connect, to a reference server and one or many DAS servers. It merges the data from all the servers, and displays sequence information as well as annotated feature information form all the available DAS Servers in a very user friendly way .                                                                                           | PERL and AJAX        | <http://www.ebi.ac.uk/dasty/>                       |

### Writing your own DAS client

#### A Java DAS Client Library - Dasobert

Examples of client code written in Java using Dasobert can be found
here: <http://www.derkholm.net/svn/repos/dasobert/trunk/doc/examples/>

There is also a tutorial for using Dasobert within eclipse that (follows
on from the Dazzle eclipse tutorial here):
[Dasobert\_Tutorial](/wiki/Dasobert_Tutorial "wikilink")

#### Example of walking a DAS source using perl

This example was kindly provided by Felix Kokocinski: You can specify a
region or let it walk through all regions if the server can supply entry
points with lengths. This is done in eg. 20 MB slices. It takes quite
some time, but works nicely.

<font color="#555500">*`#` `Example` `script` `that` `reads` `genomic`
`data` `from` `DAS` `server`*</font><font color="#555500">*`#` `using`
`a` `defined` `chunk` `size`*</font><font color="#555500">*`#` `writing`
`the` `data` `out` `to` `a` `gff` `file.`*</font>  
<font color="#555500">*`#` `fsk@sanger.ac.uk,` `2008`*</font>  
<font color="#000055">**`use`**</font>` strict;`  
<font color="#000055">**`use`**</font>` Bio::Das::Lite;`  
<font color="#000055">**`use`**</font>` Getopt::Long;`  
  
<font color="#555500">*`#default` `DAS` `server`
`adress`*</font><font color="#000055">**`my`**</font>` $server = "`[`http://das.sanger.ac.uk/das`](http://das.sanger.ac.uk/das)`";`  
<font color="#555500">*`#default` `DAS` `source`
`name`*</font><font color="#000055">**`my`**</font>` $source = 'otter_das';`  
<font color="#555500">*`#proxy`
`name`*</font><font color="#000055">**`my`**</font>` $http_proxy = `<font color="#000055">**`undef`**</font>`;`  
` `<font color="#555500">*`#genomic` `chunk` `size` `to`
`query`*</font><font color="#000055">**`my`**</font>` $max_len    = 20000000;`  
  
  
<font color="#000055">**`my`**</font>` $chromosome = `<font color="#000055">**`undef`**</font>`;`  
` `<font color="#000055">**`my`**</font>` $start      = 0;`  
<font color="#000055">**`my`**</font>` $end        = 0;`  
<font color="#000055">**`my`**</font>` $gff_file   = `<font color="#000055">**`undef`**</font>`;`  
` `<font color="#000055">**`my`**</font>` %transcripts = ();`  
  
<font color="#000055">**`my`**</font>` $type;`  
  
`&GetOptions(`  
`       'file=`<font color="#000055">**`s`**</font>`'                 => \$gff_file,`  
`       'chromosome=`<font color="#000055">**`s`**</font>`'           => \$chromosome,`  
`       'start=`<font color="#000055">**`s`**</font>`'                => \$start,`  
`       'end=`<font color="#000055">**`s`**</font>`'                  => \$end,`  
`            'server=`<font color="#000055">**`s`**</font>`'               => \$server,`  
`            'source=`<font color="#000055">**`s`**</font>`'               => \$source,`  
`      );`  
  
<font color="#555500">*`#connect` `to` `DAS`
`server`*</font><font color="#000055">**`my`**</font>` $das = connect_das("$server/$source", $http_proxy);`  
  
<font color="#555500">*`#get` `entry` `point`
`list/lengths`*</font><font color="#555500">*`#requires` `the` `DAS`
`server` `to` `support` `the` `entry-points`
`function`*</font><font color="#000055">**`my`**</font>` $chrom_lens = get_entry_points();`  
  
<font color="#000055">**`open`**</font>`(GFF, ">$gff_file") or `<font color="#000055">**`die`**</font>` "Can't `<font color="#000055">**`open`**</font>` file $gff_file.\n";`  
  
<font color="#000055">**`if`**</font>`($chromosome){`  
`  `<font color="#555500">*`#query` `specific`
`region`*</font>`  get_region($chromosome, $start, $end);`  
`}`  
<font color="#000055">**`else`**</font>`{`  
`  `<font color="#555500">*`#go` `through` `all`
`chromosomes`*</font>`  `<font color="#000055">**`foreach`**</font>` `<font color="#000055">**`my`**</font>` $chrom (`<font color="#000055">**`keys`**</font>` %$chrom_lens){`  
`   `<font color="#000055">**`print`**</font>` "getting $chrom\n";`  
`   get_region($chrom, `<font color="#000055">**`undef`**</font>`, `<font color="#000055">**`undef`**</font>`);`  
`   %transcripts = ();`  
`  }`  
`}`  
  
  
<font color="#000055">**`close`**</font>`(GFF)or `<font color="#000055">**`die`**</font>` "Can't `<font color="#000055">**`close`**</font>` file $gff_file.\n";`  
  
  
`  `<font color="#555500">*`################################################`*</font>  
  
<font color="#555500">*`#connect` `to` `DAS`
`server`*</font><font color="#000055">**`sub`**</font>` connect_das {`  
`  `<font color="#000055">**`my`**</font>` ($dsn, $proxy) = @_;`  
  
`  `<font color="#000055">**`my`**</font>` $das = Bio::Das::Lite->new({`  
`                'timeout'    => 10000,`  
`                'dsn'        => $dsn,`  
`                'http_proxy' => $proxy,`  
`               }) or `<font color="#000055">**`die`**</font>` "cant `<font color="#000055">**`connect`**</font>` to DAS server!\n";`  
  
`  `<font color="#000055">**`return`**</font>` $das;`  
`}`  
  
  
  
<font color="#555500">*`#look` `at` `the` `region`
`requested`*</font><font color="#000055">**`sub`**</font>` get_region {`  
`  `<font color="#000055">**`my`**</font>` ($chromosome, $start, $end) = @_;`  
  
`  `<font color="#000055">**`my`**</font>` $chrom_len    = $chrom_lens->{$chromosome};`  
`  `<font color="#000055">**`my`**</font>` $region       = "";`  
  
`  `<font color="#000055">**`if`**</font>`( $start and $end){`  
`    `<font color="#000055">**`if`**</font>`($start > $end){`  
`      `<font color="#000055">**`die`**</font>` "Coordinates wrong: $start > $end!\n";`  
`    }`  
`    `<font color="#000055">**`if`**</font>`( ($end - $start) <= $max_len ){`  
`      `<font color="#555500">*`#get` `entire`
`region`*</font>`      `<font color="#000055">**`my`**</font>` $region = ":".$start.",".$end;`  
`      get_transcripts($region, $chromosome);`  
`    }`  
`    `<font color="#000055">**`else`**</font>`{`  
`      go_through_chunks($start, $end, $chromosome, $chrom_len);`  
`    }`  
`  }`  
`  `<font color="#000055">**`elsif`**</font>`( $chrom_len <= $max_len ){`  
`    `<font color="#555500">*`#get` `entire`
`chromosome`*</font>`    get_transcripts($region, $chromosome);`  
`  }`  
`  `<font color="#000055">**`else`**</font>`{`  
`    go_through_chunks(1, $chrom_len, $chromosome, $chrom_len);`  
`  }`  
  
`}`  
  
  
<font color="#555500">*`#go` `through` `a` `region` `in`
`chunks`*</font><font color="#000055">**`sub`**</font>` go_through_chunks {`  
`  `<font color="#000055">**`my`**</font>` ($chunk_start, $chunk_end, $chromosome, $chrom_len) = @_;`  
  
`  `<font color="#000055">**`my`**</font>` ($region_start, $region_end);`  
`  `<font color="#000055">**`my`**</font>` %ids_seen;`  
  
`  `<font color="#555500">*`#loop` `through` `regions` `until` `all`
`is` `covered`*</font>`  `<font color="#555500">*`#keep` `track` `of`
`genes` `to` `avoid`
`duplicates!`*</font>`  `<font color="#000055">**`for`**</font>`($region_start = $chunk_start, $region_end = $region_start   $max_len;`  
`      $region_start < $chunk_end;`  
`      $region_start = $region_end   1, $region_end  = $max_len){`  
  
`    `<font color="#000055">**`if`**</font>`($region_end > $chrom_len){`  
`      $region_end = $chrom_len;`  
`    }`<font color="#000055">**`elsif`**</font>`($region_end > $chunk_end){`  
`      $region_end = $chunk_end;`  
`    }`  
`    `<font color="#000055">**`my`**</font>` $region = ":".$region_start.",".$region_end;`  
  
`    `<font color="#555500">*`#get` `all` `transcripts` `from`
`chunk`*</font>`    `<font color="#000055">**`my`**</font>` $new_ids = get_transcripts($region, $chromosome, \%ids_seen);`  
`    %ids_seen = (%ids_seen, %$new_ids);`  
`  }`  
  
`}`  
  
  
  
<font color="#555500">*`#fetch` `all` `available` `entry-points`
`(chromosomes)` `and` `their` `lengths` `from`
`server`*</font><font color="#000055">**`sub`**</font>` get_entry_points {`  
  
`  `<font color="#000055">**`my`**</font>` %chrom_lens;`  
  
`  `<font color="#000055">**`my`**</font>` $entry_points = $das->entry_points();`  
  
`  `<font color="#000055">**`foreach`**</font>` `<font color="#000055">**`my`**</font>` $k (`<font color="#000055">**`keys`**</font>` %$entry_points){`  
`   `<font color="#000055">**`foreach`**</font>` `<font color="#000055">**`my`**</font>` $l (@{$entry_points->{$k}}){`  
`       `<font color="#000055">**`foreach`**</font>` `<font color="#000055">**`my`**</font>` $segment (@{ $l->{"segment"} }){`  
`           $chrom_lens{ $segment->{"segment_id"} } = $segment->{"segment_size"};`  
`       }`  
`   }`  
`  }`  
  
`  `<font color="#000055">**`return`**</font>` \%chrom_lens;`  
`}`  
  
  
  
<font color="#555500">*`#fetch` `the` `data` `and` `process`
`it.`*</font><font color="#555500">*`#note` `that` `this` `function`
`is` `quite` `specific` `to` `the` `way` `your` `DAS` `source` `is`
`set-up.`*</font><font color="#555500">*`#the` `idea` `is` `to` `get`
`together` `all` `exons,` `etc` `that` `belong` `to` `a` `transcript`
`and` `all` `transcripts`*</font><font color="#555500">*`#that` `belong`
`to` `a`
`gene.`*</font><font color="#000055">**`sub`**</font>` get_transcripts {`  
`  `<font color="#000055">**`my`**</font>` ( $region, $chromosome, $previous_genes ) = @_;`  
  
`  `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` "have `<font color="#000055">**`chr`**</font>` $chromosome$region\n";`  
  
`  `<font color="#000055">**`my`**</font>` %genes = ();`  
`  `<font color="#000055">**`my`**</font>` %new_features = ();`  
`  `<font color="#000055">**`my`**</font>` $response = `<font color="#000055">**`undef`**</font>`;`  
` `  
`   `<font color="#555500">*`#fetch` `DAS`
`features`*</font>`  $response = $das->features({`  
`                 'segment' => $chromosome.$region,`  
`                 'type'    => $type,`  
`                });`  
  
`  `<font color="#000055">**`while`**</font>` (`<font color="#000055">**`my`**</font>` ($url, $features) = `<font color="#000055">**`each`**</font>` %$response) {`  
  
`    `<font color="#000055">**`if`**</font>`(`<font color="#000055">**`ref`**</font>` $features `<font color="#000055">**`eq`**</font>` "ARRAY"){`  
`      `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` "Received ".`<font color="#000055">**`scalar`**</font>` @$features." features.\n";`  
  
`    FEATURES:`  
`      `<font color="#000055">**`foreach`**</font>` `<font color="#000055">**`my`**</font>` $feature (@$features) {`  
  
`   `<font color="#000055">**`my`**</font>` %notes = ();`  
  
`   `<font color="#000055">**`my`**</font>` $grouphash = $feature->{'group'}->[0];`  
  
`   `<font color="#555500">*`#get` `other`
`notes`*</font>` `<font color="#000055">**`my`**</font>` $i = 0;`  
`   `<font color="#000055">**`my`**</font>` $morenote_entry = '';`  
`    `<font color="#000055">**`while`**</font>`(`<font color="#000055">**`defined`**</font>`($feature->{'note'}->[$i])){`  
`     `<font color="#000055">**`my`**</font>` $morenotes = $feature->{'note'}->[$i];`  
`     `<font color="#000055">**`my`**</font>` ($morenotes_type, $morenotes_value) = `<font color="#000055">**`split`**</font>`('=', $morenotes);`  
`     $morenotes_value =~ `<font color="#000055">**`s`**</font>`/\&\`<font color="#555500">*`#39\;/\'/g;`*</font>`      $notes{$morenotes_type} = $morenotes_value;`  
`     $i  ;`  
`   }`  
  
`   `<font color="#555500">*`#remove` `duplicates` `from` `overlapping`
`regions`*</font>`  `<font color="#000055">**`if`**</font>`(`<font color="#000055">**`defined`**</font>` $previous_genes and `<font color="#000055">**`exists`**</font>`($previous_genes->{$grouphash->{'group_type'}})){`  
`     `<font color="#000055">**`next`**</font>` FEATURES;`  
`   }`  
  
`   `<font color="#555500">*`#you` `could` `do` `some` `filtering` `of`
`the` `response` `at` `this` `point`*</font>  
`   `<font color="#000055">**`my`**</font>` %gff_element;`  
  
`   `<font color="#555500">*`#build` `structure` `for` `exons` `and`
`general` `items`*</font>` `<font color="#555500">*`#find`
`type`*</font>`   `<font color="#000055">**`my`**</font>` $element_type = $feature->{'type'} || "exon";`  
`   $element_type    =~ `<font color="#000055">**`m`**</font>`/((intron)|(UTR)|(exon))/g;`  
`   `<font color="#000055">**`if`**</font>`($1){ $element_type = $1 }`  
  
`   `<font color="#000055">**`my`**</font>` $group_type   = $grouphash->{'group_type'};`  
  
`   `<font color="#000055">**`my`**</font>` $strand       = $feature->{'orientation'};`  
`   `<font color="#000055">**`if`**</font>`($feature->{'orientation'}    =~ /^(\ |\-|\.)$/) {  }`  
`   `<font color="#000055">**`elsif`**</font>`($feature->{'orientation'} ==  1){ $strand = ' ' }`  
`   `<font color="#000055">**`elsif`**</font>`($feature->{'orientation'} == -1){ $strand = '-' }`  
`   `<font color="#000055">**`elsif`**</font>`($feature->{'orientation'} ==  0){ $strand = '.' }`  
`   `<font color="#000055">**`else`**</font>`{ `<font color="#000055">**`die`**</font>` "INVALID STRAND SYMBOL: ".$feature->{'orientation'}."\n"; }`  
  
`   `<font color="#000055">**`my`**</font>` $phase        = ".";`  
`   `<font color="#000055">**`if`**</font>`($feature->{'phase'}){`  
`     $phase = $feature->{'phase'};`  
`   }`  
`   `<font color="#000055">**`elsif`**</font>`($element_type `<font color="#000055">**`eq`**</font>` "exon"){`  
`     $phase = "0";`  
`   }`  
  
`   `<font color="#000055">**`if`**</font>`(!$notes{"Transcriptstatus"}){`  
`     `<font color="#000055">**`die`**</font>` "PROBLEM: $element_type, ".$feature->{'feature_id'}."\n";`  
`   }`  
  
`   $gff_element{'seqid'}      = $chromosome;`  
`   $gff_element{'source'}     = $notes{"Transcripttype"};`  
`   $gff_element{'type'}       = $element_type;`  
`   $gff_element{'start'}      = $feature->{'start'};`  
`   $gff_element{'end'}        = $feature->{'end'};`  
`   $gff_element{'score'}      = ".";`  
`   $gff_element{'strand'}     = $strand;`  
`   $gff_element{'phase'}      = $phase;`  
  
`   `<font color="#555500">*`#check` `for` `some` `missing`
`values`*</font>`   `<font color="#000055">**`if`**</font>`(!`<font color="#000055">**`exists`**</font>` $feature->{'feature_id'}){`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` "Missing value `<font color="#000055">**`for`**</font>` Parent-feature_id\n";`  
`     $feature->{'feature_id'} = "0";`  
`   }`  
`   `<font color="#000055">**`if`**</font>`(!`<font color="#000055">**`exists`**</font>` $notes{"Transcriptstatus"}){`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` "Missing value `<font color="#000055">**`for`**</font>` Transcriptstatus\n";`  
`     $notes{"Transcriptstatus"} = "-";`  
`   }`  
`   `<font color="#000055">**`if`**</font>`(!`<font color="#000055">**`exists`**</font>` $notes{"Created"}){`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` "Missing value `<font color="#000055">**`for`**</font>` Created\n";`  
`     $notes{"Created"} = 0;`  
`   }`  
`   `<font color="#000055">**`if`**</font>`(!`<font color="#000055">**`exists`**</font>` $notes{"Lastmod"}){`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` "Missing value `<font color="#000055">**`for`**</font>` Lastmod\n";`  
`     $notes{"Lastmod"} = 0;`  
`   }`  
`   $gff_element{'attributes'} = "Parent=".$feature->{'feature_id'}.`  
`                                ";Status=".$notes{"Transcriptstatus"}.`  
`                    ";CREATED=".$notes{"Created"}.`  
`                    ";LASTMOD=".$notes{"Lastmod"};`  
  
`   `<font color="#000055">**`if`**</font>`(!`<font color="#000055">**`exists`**</font>` $genes{ $group_type }){`  
`     $genes{ $group_type } = 1;`  
`     `<font color="#000055">**`my`**</font>` %gff_gene;`  
  
`          `<font color="#000055">**`my`**</font>` $gene_region = $feature->{'target'};`  
`          `<font color="#000055">**`my`**</font>` ($gs, $gene_loc) = `<font color="#000055">**`split`**</font>`('\=', $gene_region);`  
`     `<font color="#000055">**`my`**</font>` ($gene_start, $gene_end) = `<font color="#000055">**`split`**</font>`('\-', $gene_loc);`  
  
`     `<font color="#555500">*`#build` `structure` `for`
`gene`*</font>`    $gff_gene{'seqid'}      = $chromosome;`  
`     $gff_gene{'source'}     = $notes{"Genetype"};`  
`     $gff_gene{'type'}       = "gene";`  
`     $gff_gene{'start'}      = $gene_start;`  
`     $gff_gene{'end'}        = $gene_end;`  
`     $gff_gene{'score'}      = ".";`  
`     $gff_gene{'strand'}     = $strand;`  
`     $gff_gene{'phase'}      = ".";`  
  
`     `<font color="#555500">*`#get` `gene`
`description`*</font>`    `<font color="#000055">**`my`**</font>` $description = "";`  
`     `<font color="#000055">**`foreach`**</font>` `<font color="#000055">**`my`**</font>` $gnote (@{$grouphash->{'note'}}){`  
`       `<font color="#000055">**`my`**</font>` ($gnote_s, $gnote_string) = `<font color="#000055">**`split`**</font>`('=', $gnote);`  
`       `<font color="#000055">**`if`**</font>`($gnote_s `<font color="#000055">**`eq`**</font>` "DESCR"){`  
`         $description = ";Description=".$gnote_string;`  
`       }`  
`     }`  
`     $gff_gene{'attributes'} = "ID=".$grouphash->{'group_type'}.`  
`                               $description.`  
`                   ";Status=".$notes{"Genestatus"}.`  
`                               ";CREATED=".$notes{"Created"}.`  
`                   ";LASTMOD=".$notes{"Lastmod"};`  
  
`     `<font color="#555500">*`#print` `entry` `for`
`transcript`*</font>`      print_gff_line(\%gff_gene);`  
`     %gff_gene = ();`  
  
`     $new_features{$grouphash->{'group_type'}} = 1;`  
  
`   }`  
  
`   `<font color="#000055">**`if`**</font>`(!`<font color="#000055">**`exists`**</font>` $transcripts{ $feature->{'feature_id'} }){`  
`     $transcripts{ $feature->{'feature_id'} } = 1;`  
`     `<font color="#000055">**`my`**</font>` %gff_transcript;`  
  
`     `<font color="#555500">*`#build` `structure` `for`
`transcript`*</font>`      $gff_transcript{'seqid'}      = $chromosome;`  
`     $gff_transcript{'source'}     = $notes{"Transcripttype"};`  
`     $gff_transcript{'type'}       = "transcript";`  
`     $gff_transcript{'start'}      = $feature->{'target_start'};`  
`     $gff_transcript{'end'}        = $feature->{'target_stop'};`  
`     $gff_transcript{'score'}      = ".";`  
`     $gff_transcript{'strand'}     = $strand;`  
`     $gff_transcript{'phase'}      = ".";`  
`     $gff_transcript{'attributes'} = "ID=".$feature->{'feature_id'}.";Alias1=".$feature->{'target_id'}.`  
`                                     ";Parent=".$grouphash->{'group_type'}.`  
`                     ";CREATED=".$notes{"Created"}.`  
`                     ";LASTMOD=".$notes{"Lastmod"}.`  
`                     ";Status=".$notes{"Transcriptstatus"};`  
  
`     `<font color="#555500">*`#print` `entry` `for`
`transcript`*</font>`      print_gff_line(\%gff_transcript);`  
`     %gff_transcript = ();`  
`   }`  
`   `<font color="#555500">*`#else{` `print` `STDERR` `"_"`
`}`*</font>  
`   `<font color="#555500">*`#print` `entry` `for` `exons,`
`etc.`*</font>` `<font color="#000055">**`if`**</font>`($feature->{'type_category'} =~ /error/){`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` "Found an error feature:\n";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'seqid'}."\t";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'source'}."\t";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'type'}."\t";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'start'}."\t";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'end'}."\t";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'score'}."\t";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'strand'}."\t";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'phase'}."\t";`  
`     `<font color="#000055">**`print`**</font>` `<font color="#000055">**`STDERR`**</font>` $gff_element{'attributes'}."\n";`  
`   } `<font color="#000055">**`else`**</font>` {`  
`     print_gff_line(\%gff_element);`  
`     %gff_element = ();`  
`   }`  
  
`   $feature = `<font color="#000055">**`undef`**</font>`;`  
`       }`  
`       @$features = ();`  
`       $features  = `<font color="#000055">**`undef`**</font>`;`  
`     }`  
`   }`  
` `  
`   `<font color="#000055">**`return`**</font>` \%new_features;`  
`}`  
  
  
  
<font color="#555500">*`#print` `the` `different` `data` `types` `as`
`GFF`*</font><font color="#000055">**`sub`**</font>` print_gff_line {`  
`  `<font color="#000055">**`my`**</font>` ($element) = @_;`  
  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'seqid'}."\t";`  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'source'}."\t";`  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'type'}."\t";`  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'start'}."\t";`  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'end'}."\t";`  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'score'}."\t";`  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'strand'}."\t";`  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'phase'}."\t";`  
`  `<font color="#000055">**`print`**</font>` GFF $element->{'attributes'}."\n";`  
`}`

Acknowledgments
---------------

(some of this document may have been cut an pasted from documentation
contributed by the following people):

-   Andreas Prlic
-   Andy Jenkinson
-   Phil Jones
-   Tim Hubbard
-   Lincoln Stein
-   Thomas Down

