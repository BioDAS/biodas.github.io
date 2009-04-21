---
title: Everything DAS
permalink: wiki/Everything_DAS/
layout: wiki
---

<div id="main">
<em>Last Updated 5th Feb 2009</em>

The intention of this document is to bring together and add to all the
documentation available on the WWW for the DAS system. The content on
these pages draws from many sources of information and thus has many
contributors. Eventually the intention if that this document will be a
set of instructions that you can print out and use as reference
documentation or a good read. If you find any errors on these pages and
pages that it links to then please contact me
(<a href="mailto:jw12@sanger.ac.uk">Jonathan Warren</a>) to let me know,
any suggestions and contributions are also welcomed.

<h1>
Index:

</h1>
<h2>
<a href="#what">What is DAS?</a>

</h2>
<h3>
<a href="#currentStatus">Current Status</a>

</h3>
<h2>
<a href="#settingUp">Setting up a DAS Server</a>

</h2>
<h3>
<a href="#serversList">Servers available</a>

</h3>
<h4>
<a href="#dazzle">Dazzle</a>

</h4>
<h5>
<a href="#gettingDazzle">Getting Dazzle</a>

<h5>
<a href="#readyPlugins">Using ready made plugins for datasources</a>

</h5>
<h5>
<a href="#ownPlugins">Writing your own plugin</a>

</h5>
<h5>
<a href="#ensemblRef">Deploying an Ensembl Reference Server</a>

</h5>
<h4>
<a href="#myDas">MyDas</a>

</h4>
<h4>
<a href="#proserver">Proserver</a>

</h4>
<h3>
<a href="#impLatest">Implementing the latest specs</a>

</h3>
<h3>
<a href="#testingServer">Testing your implementation</a>

</h3>
<h3>
<a href="#validation">Validation and Registering of your Server</a>

</h3>
<h4>
<a href="#relaxNG">RelaxNG and other validation in the Registry</a>

</h4>
<h2>
<a href="#dasregistry">The DAS Registry</a>

</h2>
<h3>
<a href="#dasRegGenInfo">Introduction to the DAS Registry</a>

</h3>
<h3>
<a name="#connectingToReg">Connecting to the Registry
Programmatically</a>

</h3>
<h2>
<a href="#settingUpClient">Setting up a DAS client</a>

</h2>
<h3>
<a href="#clientsList">Currently Available DAS Clients - table?</a>

</h3>
<h3>
<a href="#writingClients">Writing your own DAS client</a>

</h3>
<h4>
<a href="#dasobert">A Java DAS Client Library - Dasobert</a>

</h4>
<h4>
<a href="#perlWalking">Example of walking a DAS source using perl</a>

</h4>
<h2>
<a href="#acknowledgements">Acknowledgments</a>

</h2>
<h1>
Content:

</h1>
<h2>
<a name="what"></a>What is DAS?

</h2>
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
from this source in the context of features from other institutions.

DAS stands for Distributed Annotation System. It was originally set up
to be used with genomic information where annotations/features are
layered on top of a reference sequence , usually a genome. The idea is
that a genome browser such as ensembl or GBrowse (both DAS clients in
this scenario) can be used to look at annotations from data sources both
that exist on the same server/machine the browser is running on and
display annotations in the same view from data sources (data served by
DAS servers) that could be on the other side of the world (communicating
via the WWW). The DAS system consists of the DAS Registry
www.dasregistry.org as well as DAS Servers and Clients. The Registry is
there to enable people and computers to easily find the DAS data sources
available around the world and also to help these data sources conform
to the specifications. It's important that data served by DAS servers
conform to enable the interoperability of different clients and servers
around the world. <a href="http://www.biodas.org/wiki/DAS1.6"> The 1.6
spec</a> is the latest and soon to be official DAS spec that mainly
focuses on genomic annotations but also refers to the
<em>E</em>xtentions specified in the 1.53<em>E</em> spec below.
<a href="http://www.dasregistry.org/spec_1.53E.jsp"> The 1.53E spec</a>
contains up to date specifications for servers and clients that support
information that can be exchanged using DAS that is not genome centric.
Types of data include Proteins- Structures and alignments, Molecular
Interactions, volume map data.

<h3>
<a name="currentStatus">Current Status/ DAS specifications 1.5, 1.53E,
1.6, 2.0 and Future Intentions</a>

</h3>
Currently DAS 1.5 is the most widely used and supported together with
1.53E. DAS 2.0 is quite different and is really running in parallel to
the other 2 versions of DAS and it is hoped that in the next few years
these versions will become one version (the 1.6 Spec includes some of
the commands from the 2.0 spec). If you wish your data to be widely
accessible then use the <a href="http://www.biodas.org/wiki/DAS1.6"> The
1.6 spec</a> and <a href="http://www.dasregistry.org/spec_1.53E.jsp">
The 1.53E spec</a> documents as their guide. If your main priority is
using the most recent technology and external libraries then
<a href="http://biodas.org/documents/das2/das2_protocol.html">DAS2.0</a>
may be of most interest to you.

<h2>
<a name="settingUp"></a>Setting up a DAS Server

</h2>
There are several different options available for setting up a DAS
server. All are either written in PERL or Java.

<h3>
<a name="serversList"></a>Servers available

</h3>
&lt;%@include file="sangertablestart.jsp"%&gt;

<tr>
<th>
Name

</th>
<th>
Programming Language

</th>
<th>
advantages

</th>
<th>
disadvantages

</th>
</tr>
<tr>
<td>
Dazzle

</td>
<td>
Java

<td>
Standard implementation, includes support for extensions (structure,
interaction, vol)

</td>
<td>
Some people say it can be hard to configure and deploy if you are not
used to Java web development

</td>
</tr>
<tr>
<td>
Proserver

</td>
<td>
PERL

</td>
<td>
Standard implementation includes support for extensions (structure,
interaction, vol)

</td>
<td>
</td>
</tr>
<tr>
<td>
MyDAS

</td>
<td>
Java

</td>
<td>
Some people say it's easier to set up and configure than Dazzle

</td>
<td>
Doesn't support extensions currently

</td>
</tr>
<tr>
<td>
LDAS

</td>
<td>
PERL

</td>
<td>
Very Easy to set up?

</td>
<td>
Limited support for DAS functionality and sources

</td>
</tr>
`     <%@include file="sangertableend.jsp" %>`

<h4>
<a name="dazzle"></a>Dazzle

</h4>
Dazzle is currently the standard/default implementation for Java users-
however MyDas (mentioned below) is popular.

<h4>
Dazzle Eclipse Tutorial

</h4>
<a href="DazzzleTutorial.jsp">Dazzle Eclipse Tutorial</a> This tutorial
takes you through setting up Dazzle in eclipse and then shows you how to
add your own plugins

<h5>
<a name="gettingDazzle"></a>Getting Dazzle

</h5>
<a href="http://biojava.org/wiki/Dazzle#Getting_Dazzle"><http://biojava.org/wiki/Dazzle#Getting_Dazzle></a>
The latest version from the cutting edge source code is available here
from subversion:<a href="http://www.derkholm.net/svn/repos/dazzle/">
<http://www.derkholm.net/svn/repos/dazzle/></a>

<h5>
<a name="readyPlugins"></a>Using ready made plugins for datasources

</h5>
<a href="http://biojava.org/wiki/Dazzle:plugins"><http://biojava.org/wiki/Dazzle:plugins></a>
<font color="red">More examples needed here and tips for using mysql
etc?</font>
<a href="http://biojava.org/wiki/Dazzle:deployment"><http://biojava.org/wiki/Dazzle:deployment></a>

<h5>
<a name="ownPlugins"></a>Writing your own plugin

</h5>
<a href="http://biojava.org/wiki/Dazzle:writeplugin"><http://biojava.org/wiki/Dazzle:writeplugin></a>  
<a href="http://biojava.org/wiki/Dazzle:eclipse"> How to write a plugin
using eclipse</a>

<font color="red">more on what interfaces need to be implemented and
give a full example that implements all needed functionality such as
sources.cmd and coordinate system etc.</font>

<h5>
<a name="ensemblRef"></a>Deploying an Ensembl Reference Server

</h5>
<a href="http://biojava.org/wiki/Dazzle:Ensembl"> link to Ensembl
reference server instructions</a>

<h4>
<a name="myDas"></a>MyDas

</h4>
<a href="http://code.google.com/p/mydas/">Information about MyDas can be
found here.</a>

<h4>
<a name="proserver"></a>Proserver

</h4>
<a href="http://www.sanger.ac.uk/Software/analysis/proserver/">Proserver
Page at the Sanger Institute.</a>  
<a href="http://proserver.svn.sourceforge.net/viewvc/proserver/trunk/doc/proserver_tutorial.html">Proserver
Tutorial</a>  
<a href="http://proserver.svn.sourceforge.net/viewvc/proserver/trunk/doc/proserver_guide.html">Guide
to Proserver</a>  

<h3>
<a name="impLatest"></a>Implementing the latest specs

</h3>
Proserver example of config to implement sources cmd:

    coordinates = TAIR_8,Chromosome,Arabidopsis thaliana -> 1:2000,3000
    properties  = key1 -> value1 ; key2 -> value2
    mapmaster   = 
    http://www.gramene.org/das/Arabidopsis_thaliana.TAIR8.reference
    capabilities = features -> 1.0

The coordinates data is taken from the
coordinates/registry\_coordinates.xml file, which is an archived copy of
the list of coordinates available in the DAS registry. Specifying the
name (or URI, actually) and test range is enough, ProServer will pick up
the rest from the XML file.

If the full data is not picked up, you may need to update the
coordinates XML file from the registry
(http://www.dasregistry.org/das/coordinatesystem). If your coordinate
system is not in the Registry, an admin can add it for you.

<h4>
<a name="ontologies"></a>Protein Annotations and Ontologies

</h4>
<a href="extension_ontology.jsp">explanation of ontologies for proteins
usage in DAS</a>

<h3>
<a name="testingServer"></a>Testing your implementation

</h3>
<h3>
<a name="validation"></a>Validation and Registering of your Server

</h3>
<h4>
<a name="relaxNG"></a>RelaxNG and other validation in the Registry

</h4>
The DAS Registry uses <a href="http://relaxng.org/">RelaxNG</a> to
validate the xml responses from DAS servers before allowing them to
register as a valid das source. RelaxNG is essentially a document like a
dtd except that it uses an xml syntax that is easy to learn quickly. The
registry uses the documents found at the following
<a href="http://www.dasregistry.org/validation/"><http://www.dasregistry.org/validation/></a>
and has one document for each of the DAS commands (note you may need to
right click "view the source" to see anything on these pages in a web
browser)
<a href="http://www.dasregistry.org/validation/features.rng">features.rng</a>,
<a href="http://www.dasregistry.org/validation/sources.rng">sources.rng</a>,
<a href="http://www.dasregistry.org/validation/alignments.rng">alignments.rng</a>,
<a href="http://www.dasregistry.org/validation/structure.rng">structure.rng</a>,
<a href="http://www.dasregistry.org/validation/entry_points.rng">entry\_points.rng</a>,
<a href="http://www.dasregistry.org/validation/interaction.rng">interaction.rng</a>,
<a href="http://www.dasregistry.org/validation/sequence.rng">sequence.rng</a>
and
<a href="http://www.dasregistry.org/validation/types.rng">types.rng</a>.

<h2>
<a name="dasregistry"></a>The DAS Registry

</h2>
<h3>
<a name="dasRegGenInfo"></a>Introduction to the DAS Registry

</h3>
<h3>
<a name="connectingToReg"></a>Connecting to the Registry
Programmatically

</h3>
There are several commands that can be used to query the registry
including: The sources cmd with optional parameters: label, organism,
authority, capability, type and unique source\_id. You can also use the
organsim, coordinatesystem and lastmodified commands. For examples see
<a href="http://www.dasregistry.org/help_scripting.jsp">Scripting</a> an
example of a java classe written using Dasobert to access the Registry
is here
<a href="http://www.derkholm.net/svn/repos/dasobert/trunk/doc/examples/ContactRegistry.java"><http://www.derkholm.net/svn/repos/dasobert/trunk/doc/examples/ContactRegistry.java></a>

<h2>
<a name="settingUpClient"></a>Setting up a DAS client

</h2>
<h3>
<a name="clientsList"></a>Currently Available DAS Clients - table?

</h3>
&lt;%@include file="sangertablestart.jsp"%&gt;

<tr>
<th>
Name

</th>
<th>
Description

<th>
Programming Language

</th>
<th>
Links

</th>
</tr>
<tr>
<td>
GBrowse

</td>
<td>
quote from GBrowse Website "GBrowse\[1\] is the most popular viewer in
GMOD. For a list of GBrowse and GMOD installations see the GMOD Users
page. For a demo of its features, try the
<a href="http://www.wormbase.org/db/seq/gbrowse/wormbase/">WormBase</a>,
<a href="http://flybase.org/cgi-bin/gbrowse/dmel">FlyBase</a>, or
<a href="http://projects.tcag.ca/cgi-bin/duplication/dupbrowse/human_b35">Human
Genome Segmental Duplication Database</a> web sites. Spec DAS 1.53E and
1.6 soon

</td>
<td>
PERL

</td>
<td>
<a href="http://gmod.org/wiki/Gbrowse"><http://gmod.org/wiki/Gbrowse></a>

</td>
</tr>
<tr>
<td>
EnsEMBL

</td>
<td>
EnsEMBL is a web based genome browser and database system which supports
DAS 1.53E and soon 1.6

</td>
<td>
PERL

</td>
<td>
<a href="http://www.ensembl.org"><http://www.ensembl.org/></a>

</td>
</tr>
<tr>
<td>
IGB

</td>
<td>
is an application built upon the GenoViz SDK and Genometry for
visualization and exploration of genomes and corresponding annotations
from multiple data sources

</td>
<td>
Java

</td>
<td>
&lt;a
<http://genoviz.sourceforge.net/>"&gt;http://genoviz.sourceforge.net/</a>

</td>
</tr>
<tr>
<td>
Jalview

</td>
<td>
A multiple sequence alignment editor & viewer

<td>
Java

</td>
<td>
<a href="http://www.jalview.org/"><http://www.jalview.org/></a>

</td>
</tr>
<tr>
<td>
Dasty2

</td>
<td>
Dasty, a protein DAS client is implemented for visualising protein
sequence feature information. The client is able to connect, to a
reference server and one or many DAS servers. It merges the data from
all the servers, and displays sequence information as well as annotated
feature information form all the available DAS Servers in a very user
friendly way .

<td>
PERL and AJAX

</td>
<td>
<a href="http://www.ebi.ac.uk/dasty/"><http://www.ebi.ac.uk/dasty/></a>

</td>
</tr>
`     `<font color="red">`Add clients from the workshop`</font>  
`     <%@include file="sangertableend.jsp" %>`

<h3>
<a name="writingClients"></a>Writing your own DAS client

</h3>
<h4>
<a name="dasobert"></a>A Java DAS Client Library - Dasobert

</h4>
Examples of client code written in Java using Dasobert can be found
here:
<a href="http://www.derkholm.net/svn/repos/dasobert/trunk/doc/examples/"><http://www.derkholm.net/svn/repos/dasobert/trunk/doc/examples/></a>

There is also a tutorial for using Dasobert within eclipse that (follows
on from the Dazzle eclipse tutorial here):
<a href="DasobertTutorial.jsp">Dasobert Eclipse Tutorial</a>

<h4>
<a name="perlWalking"></a>Example of walking a DAS source using perl

</h4>
This example was kindly provided by Felix Kokocinski:

You can specify a region or let it walk through all regions if the
server can supply entry points with lengths. This is done in eg. 20 MB
slices. It takes quite some time, but works nicely.

    <FONT COLOR="#555500"><I># Example script that reads genomic data from DAS server
    </I></FONT><FONT COLOR="#555500"><I># using a defined chunk size
    </I></FONT><FONT COLOR="#555500"><I># writing the data out to a gff file.
    </I></FONT>
    <FONT COLOR="#555500"><I># fsk@sanger.ac.uk, 2008
    </I></FONT>
    <FONT COLOR="#000055"><B>use</B></FONT> strict;
    <FONT COLOR="#000055"><B>use</B></FONT> Bio::Das::Lite;
    <FONT COLOR="#000055"><B>use</B></FONT> Getopt::Long;

    <FONT COLOR="#555500"><I>#default DAS server adress

    </I></FONT><FONT COLOR="#000055"><B>my</B></FONT> $server = "http://das.sanger.ac.uk/das";
    <FONT COLOR="#555500"><I>#default DAS source name
    </I></FONT><FONT COLOR="#000055"><B>my</B></FONT> $source = 'otter_das';
    <FONT COLOR="#555500"><I>#proxy name
    </I></FONT><FONT COLOR="#000055"><B>my</B></FONT> $http_proxy = <FONT COLOR="#000055"><B>undef</B></FONT>;
    <FONT COLOR="#555500"><I>#genomic chunk size to query
    </I></FONT><FONT COLOR="#000055"><B>my</B></FONT> $max_len    = 20000000;


    <FONT COLOR="#000055"><B>my</B></FONT> $chromosome = <FONT COLOR="#000055"><B>undef</B></FONT>;
    <FONT COLOR="#000055"><B>my</B></FONT> $start      = 0;
    <FONT COLOR="#000055"><B>my</B></FONT> $end        = 0;
    <FONT COLOR="#000055"><B>my</B></FONT> $gff_file   = <FONT COLOR="#000055"><B>undef</B></FONT>;
    <FONT COLOR="#000055"><B>my</B></FONT> %transcripts = ();

    <FONT COLOR="#000055"><B>my</B></FONT> $type;

    &amp;GetOptions(
            'file=<FONT COLOR="#000055"><B>s</B></FONT>'                 => \$gff_file,
            'chromosome=<FONT COLOR="#000055"><B>s</B></FONT>'           => \$chromosome,
            'start=<FONT COLOR="#000055"><B>s</B></FONT>'                => \$start,
            'end=<FONT COLOR="#000055"><B>s</B></FONT>'                  => \$end,
                'server=<FONT COLOR="#000055"><B>s</B></FONT>'               => \$server,
                'source=<FONT COLOR="#000055"><B>s</B></FONT>'               => \$source,
           );

    <FONT COLOR="#555500"><I>#connect to DAS server

    </I></FONT><FONT COLOR="#000055"><B>my</B></FONT> $das = connect_das("$server/$source", $http_proxy);

    <FONT COLOR="#555500"><I>#get entry point list/lengths
    </I></FONT><FONT COLOR="#555500"><I>#requires the DAS server to support the entry-points function
    </I></FONT><FONT COLOR="#000055"><B>my</B></FONT> $chrom_lens = get_entry_points();

    <FONT COLOR="#000055"><B>open</B></FONT>(GFF, ">$gff_file") or <FONT COLOR="#000055"><B>die</B></FONT> "Can't <FONT COLOR="#000055"><B>open</B></FONT> file $gff_file.\n";

    <FONT COLOR="#000055"><B>if</B></FONT>($chromosome){
      <FONT COLOR="#555500"><I>#query specific region

    </I></FONT>  get_region($chromosome, $start, $end);
    }
    <FONT COLOR="#000055"><B>else</B></FONT>{
      <FONT COLOR="#555500"><I>#go through all chromosomes  
    </I></FONT>  <FONT COLOR="#000055"><B>foreach</B></FONT> <FONT COLOR="#000055"><B>my</B></FONT> $chrom (<FONT COLOR="#000055"><B>keys</B></FONT> %$chrom_lens){
        <FONT COLOR="#000055"><B>print</B></FONT> "getting $chrom\n";
        get_region($chrom, <FONT COLOR="#000055"><B>undef</B></FONT>, <FONT COLOR="#000055"><B>undef</B></FONT>);
        %transcripts = ();
      }
    }


    <FONT COLOR="#000055"><B>close</B></FONT>(GFF)or <FONT COLOR="#000055"><B>die</B></FONT> "Can't <FONT COLOR="#000055"><B>close</B></FONT> file $gff_file.\n";


      <FONT COLOR="#555500"><I>################################################
    </I></FONT>

    <FONT COLOR="#555500"><I>#connect to DAS server
    </I></FONT><FONT COLOR="#000055"><B>sub</B></FONT> connect_das {
      <FONT COLOR="#000055"><B>my</B></FONT> ($dsn, $proxy) = @_;

      <FONT COLOR="#000055"><B>my</B></FONT> $das = Bio::Das::Lite->new({
                     'timeout'    => 10000,
                     'dsn'        => $dsn,
                     'http_proxy' => $proxy,
                    }) or <FONT COLOR="#000055"><B>die</B></FONT> "cant <FONT COLOR="#000055"><B>connect</B></FONT> to DAS server!\n";

      <FONT COLOR="#000055"><B>return</B></FONT> $das;
    }



    <FONT COLOR="#555500"><I>#look at the region requested
    </I></FONT><FONT COLOR="#000055"><B>sub</B></FONT> get_region {
      <FONT COLOR="#000055"><B>my</B></FONT> ($chromosome, $start, $end) = @_;

      <FONT COLOR="#000055"><B>my</B></FONT> $chrom_len    = $chrom_lens->{$chromosome};
      <FONT COLOR="#000055"><B>my</B></FONT> $region       = "";

      <FONT COLOR="#000055"><B>if</B></FONT>( $start and $end){
        <FONT COLOR="#000055"><B>if</B></FONT>($start > $end){
          <FONT COLOR="#000055"><B>die</B></FONT> "Coordinates wrong: $start > $end!\n";
        }
        <FONT COLOR="#000055"><B>if</B></FONT>( ($end - $start) &lt;= $max_len ){
          <FONT COLOR="#555500"><I>#get entire region

    </I></FONT>      <FONT COLOR="#000055"><B>my</B></FONT> $region = ":".$start.",".$end;
          get_transcripts($region, $chromosome);
        }
        <FONT COLOR="#000055"><B>else</B></FONT>{
          go_through_chunks($start, $end, $chromosome, $chrom_len);
        }
      }
      <FONT COLOR="#000055"><B>elsif</B></FONT>( $chrom_len &lt;= $max_len ){
        <FONT COLOR="#555500"><I>#get entire chromosome
    </I></FONT>    get_transcripts($region, $chromosome);
      }
      <FONT COLOR="#000055"><B>else</B></FONT>{
        go_through_chunks(1, $chrom_len, $chromosome, $chrom_len);
      }

    }


    <FONT COLOR="#555500"><I>#go through a region in chunks
    </I></FONT><FONT COLOR="#000055"><B>sub</B></FONT> go_through_chunks {
      <FONT COLOR="#000055"><B>my</B></FONT> ($chunk_start, $chunk_end, $chromosome, $chrom_len) = @_;

      <FONT COLOR="#000055"><B>my</B></FONT> ($region_start, $region_end);
      <FONT COLOR="#000055"><B>my</B></FONT> %ids_seen;

      <FONT COLOR="#555500"><I>#loop through regions until all is covered

    </I></FONT>  <FONT COLOR="#555500"><I>#keep track of genes to avoid duplicates!
    </I></FONT>  <FONT COLOR="#000055"><B>for</B></FONT>($region_start = $chunk_start, $region_end = $region_start + $max_len;
          $region_start &lt; $chunk_end;
          $region_start = $region_end + 1, $region_end += $max_len){

        <FONT COLOR="#000055"><B>if</B></FONT>($region_end > $chrom_len){
          $region_end = $chrom_len;
        }<FONT COLOR="#000055"><B>elsif</B></FONT>($region_end > $chunk_end){
          $region_end = $chunk_end;
        }
        <FONT COLOR="#000055"><B>my</B></FONT> $region = ":".$region_start.",".$region_end;

        <FONT COLOR="#555500"><I>#get all transcripts from chunk
    </I></FONT>    <FONT COLOR="#000055"><B>my</B></FONT> $new_ids = get_transcripts($region, $chromosome, \%ids_seen);
        %ids_seen = (%ids_seen, %$new_ids);
      }

    }



    <FONT COLOR="#555500"><I>#fetch all available entry-points (chromosomes) and their lengths from server
    </I></FONT><FONT COLOR="#000055"><B>sub</B></FONT> get_entry_points {

      <FONT COLOR="#000055"><B>my</B></FONT> %chrom_lens;

      <FONT COLOR="#000055"><B>my</B></FONT> $entry_points = $das->entry_points();

      <FONT COLOR="#000055"><B>foreach</B></FONT> <FONT COLOR="#000055"><B>my</B></FONT> $k (<FONT COLOR="#000055"><B>keys</B></FONT> %$entry_points){
        <FONT COLOR="#000055"><B>foreach</B></FONT> <FONT COLOR="#000055"><B>my</B></FONT> $l (@{$entry_points->{$k}}){
            <FONT COLOR="#000055"><B>foreach</B></FONT> <FONT COLOR="#000055"><B>my</B></FONT> $segment (@{ $l->{"segment"} }){
                $chrom_lens{ $segment->{"segment_id"} } = $segment->{"segment_size"};
            }
        }
      }

      <FONT COLOR="#000055"><B>return</B></FONT> \%chrom_lens;
    }



    <FONT COLOR="#555500"><I>#fetch the data and process it.
    </I></FONT><FONT COLOR="#555500"><I>#note that this function is quite specific to the way your DAS source is set-up.
    </I></FONT><FONT COLOR="#555500"><I>#the idea is to get together all exons, etc that belong to a transcript and all transcripts
    </I></FONT><FONT COLOR="#555500"><I>#that belong to a gene.
    </I></FONT><FONT COLOR="#000055"><B>sub</B></FONT> get_transcripts {
      <FONT COLOR="#000055"><B>my</B></FONT> ( $region, $chromosome, $previous_genes ) = @_;

      <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> "have <FONT COLOR="#000055"><B>chr</B></FONT> $chromosome$region\n";

      <FONT COLOR="#000055"><B>my</B></FONT> %genes = ();
      <FONT COLOR="#000055"><B>my</B></FONT> %new_features = ();
      <FONT COLOR="#000055"><B>my</B></FONT> $response = <FONT COLOR="#000055"><B>undef</B></FONT>;

      <FONT COLOR="#555500"><I>#fetch DAS features

    </I></FONT>  $response = $das->features({
                      'segment' => $chromosome.$region,
                      'type'    => $type,
                     });

      <FONT COLOR="#000055"><B>while</B></FONT> (<FONT COLOR="#000055"><B>my</B></FONT> ($url, $features) = <FONT COLOR="#000055"><B>each</B></FONT> %$response) {

        <FONT COLOR="#000055"><B>if</B></FONT>(<FONT COLOR="#000055"><B>ref</B></FONT> $features <FONT COLOR="#000055"><B>eq</B></FONT> "ARRAY"){
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> "Received ".<FONT COLOR="#000055"><B>scalar</B></FONT> @$features." features.\n";

        FEATURES:
          <FONT COLOR="#000055"><B>foreach</B></FONT> <FONT COLOR="#000055"><B>my</B></FONT> $feature (@$features) {

        <FONT COLOR="#000055"><B>my</B></FONT> %notes = ();

        <FONT COLOR="#000055"><B>my</B></FONT> $grouphash = $feature->{'group'}->[0];

        <FONT COLOR="#555500"><I>#get other notes

    </I></FONT> <FONT COLOR="#000055"><B>my</B></FONT> $i = 0;
        <FONT COLOR="#000055"><B>my</B></FONT> $morenote_entry = '';
        <FONT COLOR="#000055"><B>while</B></FONT>(<FONT COLOR="#000055"><B>defined</B></FONT>($feature->{'note'}->[$i])){
          <FONT COLOR="#000055"><B>my</B></FONT> $morenotes = $feature->{'note'}->[$i];
          <FONT COLOR="#000055"><B>my</B></FONT> ($morenotes_type, $morenotes_value) = <FONT COLOR="#000055"><B>split</B></FONT>('=', $morenotes);
          $morenotes_value =~ <FONT COLOR="#000055"><B>s</B></FONT>/\&amp;\<FONT COLOR="#555500"><I>#39\;/\'/g;

    </I></FONT>   $notes{$morenotes_type} = $morenotes_value;
          $i++;
        }

        <FONT COLOR="#555500"><I>#remove duplicates from overlapping regions
    </I></FONT> <FONT COLOR="#000055"><B>if</B></FONT>(<FONT COLOR="#000055"><B>defined</B></FONT> $previous_genes and <FONT COLOR="#000055"><B>exists</B></FONT>($previous_genes->{$grouphash->{'group_type'}})){
          <FONT COLOR="#000055"><B>next</B></FONT> FEATURES;
        }

        <FONT COLOR="#555500"><I>#you could do some filtering of the response at this point
    </I></FONT>
        <FONT COLOR="#000055"><B>my</B></FONT> %gff_element;

        <FONT COLOR="#555500"><I>#build structure for exons and general items

    </I></FONT> <FONT COLOR="#555500"><I>#find type
    </I></FONT> <FONT COLOR="#000055"><B>my</B></FONT> $element_type = $feature->{'type'} || "exon";
        $element_type    =~ <FONT COLOR="#000055"><B>m</B></FONT>/((intron)|(UTR)|(exon))/g;
        <FONT COLOR="#000055"><B>if</B></FONT>($1){ $element_type = $1 }

        <FONT COLOR="#000055"><B>my</B></FONT> $group_type   = $grouphash->{'group_type'};

        <FONT COLOR="#000055"><B>my</B></FONT> $strand       = $feature->{'orientation'};
        <FONT COLOR="#000055"><B>if</B></FONT>($feature->{'orientation'}    =~ /^(\+|\-|\.)$/) {  }
        <FONT COLOR="#000055"><B>elsif</B></FONT>($feature->{'orientation'} ==  1){ $strand = '+' }
        <FONT COLOR="#000055"><B>elsif</B></FONT>($feature->{'orientation'} == -1){ $strand = '-' }
        <FONT COLOR="#000055"><B>elsif</B></FONT>($feature->{'orientation'} ==  0){ $strand = '.' }
        <FONT COLOR="#000055"><B>else</B></FONT>{ <FONT COLOR="#000055"><B>die</B></FONT> "INVALID STRAND SYMBOL: ".$feature->{'orientation'}."\n"; }

        <FONT COLOR="#000055"><B>my</B></FONT> $phase        = ".";
        <FONT COLOR="#000055"><B>if</B></FONT>($feature->{'phase'}){
          $phase = $feature->{'phase'};
        }
        <FONT COLOR="#000055"><B>elsif</B></FONT>($element_type <FONT COLOR="#000055"><B>eq</B></FONT> "exon"){
          $phase = "0";
        }

        <FONT COLOR="#000055"><B>if</B></FONT>(!$notes{"Transcriptstatus"}){
          <FONT COLOR="#000055"><B>die</B></FONT> "PROBLEM: $element_type, ".$feature->{'feature_id'}."\n";
        }

        $gff_element{'seqid'}      = $chromosome;
        $gff_element{'source'}     = $notes{"Transcripttype"};
        $gff_element{'type'}       = $element_type;
        $gff_element{'start'}      = $feature->{'start'};
        $gff_element{'end'}        = $feature->{'end'};
        $gff_element{'score'}      = ".";
        $gff_element{'strand'}     = $strand;
        $gff_element{'phase'}      = $phase;

        <FONT COLOR="#555500"><I>#check for some missing values

    </I></FONT> <FONT COLOR="#000055"><B>if</B></FONT>(!<FONT COLOR="#000055"><B>exists</B></FONT> $feature->{'feature_id'}){
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> "Missing value <FONT COLOR="#000055"><B>for</B></FONT> Parent-feature_id\n";
          $feature->{'feature_id'} = "0";
        }
        <FONT COLOR="#000055"><B>if</B></FONT>(!<FONT COLOR="#000055"><B>exists</B></FONT> $notes{"Transcriptstatus"}){
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> "Missing value <FONT COLOR="#000055"><B>for</B></FONT> Transcriptstatus\n";
          $notes{"Transcriptstatus"} = "-";
        }
        <FONT COLOR="#000055"><B>if</B></FONT>(!<FONT COLOR="#000055"><B>exists</B></FONT> $notes{"Created"}){
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> "Missing value <FONT COLOR="#000055"><B>for</B></FONT> Created\n";
          $notes{"Created"} = 0;
        }
        <FONT COLOR="#000055"><B>if</B></FONT>(!<FONT COLOR="#000055"><B>exists</B></FONT> $notes{"Lastmod"}){
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> "Missing value <FONT COLOR="#000055"><B>for</B></FONT> Lastmod\n";
          $notes{"Lastmod"} = 0;
        }
        $gff_element{'attributes'} = "Parent=".$feature->{'feature_id'}.
                                     ";Status=".$notes{"Transcriptstatus"}.
                         ";CREATED=".$notes{"Created"}.
                         ";LASTMOD=".$notes{"Lastmod"};

        <FONT COLOR="#000055"><B>if</B></FONT>(!<FONT COLOR="#000055"><B>exists</B></FONT> $genes{ $group_type }){
          $genes{ $group_type } = 1;
          <FONT COLOR="#000055"><B>my</B></FONT> %gff_gene;

              <FONT COLOR="#000055"><B>my</B></FONT> $gene_region = $feature->{'target'};
              <FONT COLOR="#000055"><B>my</B></FONT> ($gs, $gene_loc) = <FONT COLOR="#000055"><B>split</B></FONT>('\=', $gene_region);
          <FONT COLOR="#000055"><B>my</B></FONT> ($gene_start, $gene_end) = <FONT COLOR="#000055"><B>split</B></FONT>('\-', $gene_loc);

          <FONT COLOR="#555500"><I>#build structure for gene

    </I></FONT>   $gff_gene{'seqid'}      = $chromosome;
          $gff_gene{'source'}     = $notes{"Genetype"};
          $gff_gene{'type'}       = "gene";
          $gff_gene{'start'}      = $gene_start;
          $gff_gene{'end'}        = $gene_end;
          $gff_gene{'score'}      = ".";
          $gff_gene{'strand'}     = $strand;
          $gff_gene{'phase'}      = ".";

          <FONT COLOR="#555500"><I>#get gene description
    </I></FONT>   <FONT COLOR="#000055"><B>my</B></FONT> $description = "";
          <FONT COLOR="#000055"><B>foreach</B></FONT> <FONT COLOR="#000055"><B>my</B></FONT> $gnote (@{$grouphash->{'note'}}){
            <FONT COLOR="#000055"><B>my</B></FONT> ($gnote_s, $gnote_string) = <FONT COLOR="#000055"><B>split</B></FONT>('=', $gnote);
            <FONT COLOR="#000055"><B>if</B></FONT>($gnote_s <FONT COLOR="#000055"><B>eq</B></FONT> "DESCR"){
              $description = ";Description=".$gnote_string;
            }
          }
          $gff_gene{'attributes'} = "ID=".$grouphash->{'group_type'}.
                                    $description.
                        ";Status=".$notes{"Genestatus"}.
                                    ";CREATED=".$notes{"Created"}.
                        ";LASTMOD=".$notes{"Lastmod"};

          <FONT COLOR="#555500"><I>#print entry for transcript

    </I></FONT>   print_gff_line(\%gff_gene);
          %gff_gene = ();

          $new_features{$grouphash->{'group_type'}} = 1;

        }

        <FONT COLOR="#000055"><B>if</B></FONT>(!<FONT COLOR="#000055"><B>exists</B></FONT> $transcripts{ $feature->{'feature_id'} }){
          $transcripts{ $feature->{'feature_id'} } = 1;
          <FONT COLOR="#000055"><B>my</B></FONT> %gff_transcript;

          <FONT COLOR="#555500"><I>#build structure for transcript
    </I></FONT>   $gff_transcript{'seqid'}      = $chromosome;
          $gff_transcript{'source'}     = $notes{"Transcripttype"};
          $gff_transcript{'type'}       = "transcript";
          $gff_transcript{'start'}      = $feature->{'target_start'};
          $gff_transcript{'end'}        = $feature->{'target_stop'};
          $gff_transcript{'score'}      = ".";
          $gff_transcript{'strand'}     = $strand;
          $gff_transcript{'phase'}      = ".";
          $gff_transcript{'attributes'} = "ID=".$feature->{'feature_id'}.";Alias1=".$feature->{'target_id'}.
                                          ";Parent=".$grouphash->{'group_type'}.
                          ";CREATED=".$notes{"Created"}.
                          ";LASTMOD=".$notes{"Lastmod"}.
                          ";Status=".$notes{"Transcriptstatus"};

          <FONT COLOR="#555500"><I>#print entry for transcript
    </I></FONT>   print_gff_line(\%gff_transcript);
          %gff_transcript = ();
        }
        <FONT COLOR="#555500"><I>#else{ print STDERR "_" }

    </I></FONT>
        <FONT COLOR="#555500"><I>#print entry for exons, etc.
    </I></FONT> <FONT COLOR="#000055"><B>if</B></FONT>($feature->{'type_category'} =~ /error/){
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> "Found an error feature:\n";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'seqid'}."\t";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'source'}."\t";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'type'}."\t";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'start'}."\t";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'end'}."\t";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'score'}."\t";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'strand'}."\t";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'phase'}."\t";
          <FONT COLOR="#000055"><B>print</B></FONT> <FONT COLOR="#000055"><B>STDERR</B></FONT> $gff_element{'attributes'}."\n";
        } <FONT COLOR="#000055"><B>else</B></FONT> {
          print_gff_line(\%gff_element);
          %gff_element = ();
        }

        $feature = <FONT COLOR="#000055"><B>undef</B></FONT>;
          }
          @$features = ();
          $features  = <FONT COLOR="#000055"><B>undef</B></FONT>;
        }
      }

      <FONT COLOR="#000055"><B>return</B></FONT> \%new_features;
    }



    <FONT COLOR="#555500"><I>#print the different data types as GFF
    </I></FONT><FONT COLOR="#000055"><B>sub</B></FONT> print_gff_line {
      <FONT COLOR="#000055"><B>my</B></FONT> ($element) = @_;

      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'seqid'}."\t";
      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'source'}."\t";
      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'type'}."\t";
      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'start'}."\t";
      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'end'}."\t";
      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'score'}."\t";
      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'strand'}."\t";
      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'phase'}."\t";
      <FONT COLOR="#000055"><B>print</B></FONT> GFF $element->{'attributes'}."\n";
    }

<h2>
<a href="acknoledgments"></a>Acknowledgments

</h2>
(some of this document may have been cut an pasted from documentation
contributed by the following people):

-   Andreas Prlic
-   Andy Jenkinson
-   Phil Jones
-   Tim Hubbard
-   Lincoln Stein
-   Thomas Down

</div>

