---
title: ProServer/Tutorial
permalink: wiki/ProServer/Tutorial/
layout: wiki
---

This document is intended to be a quick guide to setting up ProServer to
work with a custom set of data such as you may have. The examples uses
data from a custom tab-separated flat file, but the tutorial may be
equally useful as a starting point for those wishing to expose data from
other sources, such as relational databases.

The tutorial assumes you are familiar with Perl and are operating on a
Linux platform.

Basic Architecture
------------------

ProServer is a standalone server, meaning you do not need to run a
separate web server such as Apache. It handles all of the
communications, query parsing and XML output functions, only requiring
you to:

1.  Adapt your own data to the DAS protocol.
2.  Provide the appropriate metadata configuration.

Each data source is represented in ProServer by an instance of a plugin
module. Simple data sources, especially those based on files, can often
be set up without requiring any code at all by using a pre-existing
plugin. More complex data sources may require you to write your own
plugin. This is done by creating a subclass of the
Bio::Das::ProServer::SourceAdaptor module.

The contract of a SourceAdaptor is to provide the data for a DAS query
in a data structure that the ProServer core can understand. This is done
by implementing a single method for each DAS command. For example, a DAS
source that is to respond to the 'features' command implements the
'build\_features' method, which returns an array of hashes. Each hash
represents a single feature.

ProServer includes various transport modules that exist to make
accessing your data easier by reducing the boilerplate code you need to
write. For example, the dbi transport for relational databases handles
all database connections, statements, results sets etc. Transports also
exist for flat files, SRS, the BioPerl and Ensembl APIs, etc. Unlike
SourceAdaptors, you do not nave to use a Transport if you do not want
to.

Procedure
---------

The lifecycle of a typical DAS features request is as follows:

1.  Client issues request.
2.  Core parses and checks request content.
3.  Core obtains the data source's SourceAdaptor object.
4.  Core passes the extracted query parameters to the SourceAdaptor
    object via the das\_features method.
5.  SourceAdaptor handles basic logic/iteration and delegates to the the
    build\_features method (implemented in subclass).
6.  SourceAdaptor subclass extracts the relevant data from storage and
    returns a uniform Perl data structure.
7.  SourceAdaptor constructs an XML response and passes it back to
    the core.
8.  Core sends the response back to the client.

Downloading and Building ProServer
----------------------------------

The best way to get ProServer is via the Subversion repository. The
trunk always contains the latest stable version, so includes the latest
bugfixes. To download it, open a terminal and type the following:

`svn checkout `[`http://proserver.svn.sf.net/svnroot/proserver/trunk`](http://proserver.svn.sf.net/svnroot/proserver/trunk)` Bio-Das-ProServer`

When the download is complete, enter the Bio-Das-ProServer directory
that was created and take a moment to read the README file. Proceed to
build ProServer as per the instructions. You do not need to run the make
install step (which integrates the library into the Perl installation)
as you will be working inside the Bio-Das-ProServer directory.

Running ProServer
-----------------

Although ProServer is technically a framework, the distribution contains
an example Perl script called proserver that you should use to run
proserver. It is in the eg directory. During development, you should run
this script with the -x option. This prevents the process from forking
and directs log output to your terminal rather than to file. Try running
the script in your terminal:

eg/proserver -x -c eg/proserver.ini

If all is well, the server will start and output some information about
its (default) configuration. If not, you should be able to diagnose the
problem. Commonly errors arise from:

-   The Perl interpreter being installed somewhere other
    than /usr/local/bin/perl. Edit the script with the correct location.
-   The ProServer libraries cannot be found. Since you did not install
    them into the Perl distribution, you need to be running the
    proserver script from a location where it can find the modules. It
    is looking in ./blib/lib (where the modules reside when ProServer is
    built), so make sure you run the script from the root
    proserver directory.

INI files
---------

ProServer uses an INI file to configure itself, which you specify using
the '-c' command-line option. This INI file defines lots of things such
as the port number the server should listen on, the root directory to
look for static content, and details of the DAS sources it is serving.
You will write your own INI file, but for now take a quick look at the
example proserver.ini. There are some comments describing the various
options.

Each section of the INI file is denoted by square brackets. Server
options such as port number are in the \[general\] section. All other
sections are treated as DAS sources that the server hosts, each
representing an individual source of data. Though each server can host
several sources, you will define only one. Create a new file
'eg/tutorial.ini' with this content:

`[mysource]`  
`state        = on`  
`adaptor      = myplugin`

This file configures ProServer with a DAS source called 'mysource' using
the 'Bio::Das::ProServer::SourceAdaptor::myplugin' adaptor, and turns it
on. Now start ProServer with this file instead of the example one using
the -c option:

`eg/proserver -x -c eg/tutorial.ini`

By default, ProServer listens for HTTP requests on port 9000. Open a web
browser to the URL "<http://localhost:9000/das/sources>". This runs the
sources server command, which returns an XML document listing the DAS
sources the server is hosting.

XSL Stylesheets
---------------

ProServer makes use of XSL stylesheets. Not to be confused with DAS
stylesheets (which define the glyphs a DAS client should use to draw
annotations), an XSL stylesheet is a set of instructions for converting
XML files to other formats. Modern web broswers will automatically use
these to transform the XML into a more human readable HTML format.

If you get some sort of error in your web browser at this point (e.g.
"XML Parsing Error: no element found") it is probably because ProServer
can't find its default XSL stylesheets. Since you haven't written any
configuration to tell it where to find them, the code tries to guess the
location and assumes you are running ProServer from its root directory.

To see the XML itself, use the 'view source' function of your browser.
Though your 'mytutorial' source should be listed, you will see that it
is not. Check your terminal window for errors to find out why. You will
see that ProServer attempted to build a
Bio::Das::ProServer::SourceAdaptor::tutorial object, but errored trying
to locate the module. Of course, no such module exists because you
haven't written it yet...

Configuring a SourceAdaptor
---------------------------

Let's try using a plugin that does exist: the file adaptor. Take a look
at the plugin's documentation to find out how to configure it in your
INI file:

`perldoc Bio::Das::ProServer::SourceAdaptor::file`

As you can see, this plugin allows you to use a file in order to create
a data source that supports the DAS features command. Download this
example file and save it somewhere. Now change your tutorial.ini to use
the file adaptor and tell it the location of the file you downloaded.

`[mysource]`  
`state        = on`  
`adaptor      = file`  
`filename     = /path/to/clones.txt`

You also need to tell the file SourceAdaptor the order of the columns in
the file. The column names will be used in the data structure that is
returned by the build\_features method, so you must use values that
ProServer expects. Look at the POD for the build\_features method of the
Bio::Das::ProServer::SourceAdaptor module for a list. The columns
present in the file are:

segment ID, start position, end position, strand, feature ID

`[mysource]`  
`state        = on`  
`adaptor      = file`  
`filename     = /path/to/clones.txt`  
`cols         = segment,start,end,ori,id`

The next step is to tell the adaptor exactly how to select relevant rows
from the file depending on the query segment sent to the server in a
features request. This is done by setting the feature\_query INI
property. Using the POD for Bio::Das::ProServer::SourceAdaptor::file and
Bio::Das::ProServer::SourceAdaptor::Transport::file (the transport used
by the adaptor), construct a query that will select feature rows from
the file that at least partially overlap with the query segment:

`[mysource]`  
`state        = on`  
`adaptor      = file`  
`filename     = /path/to/clones.txt`  
`cols         = segment,start,end,ori,id`  
`feature_query= field0 = %segment and field2 >= %start and field1 <= %end`

It is now time to start your server up again. You should see your source
in the server's response to the sources command:

[`http://localhost:9000/das/sources`](http://localhost:9000/das/sources)

Your source should appear in the list. The table has columns for extra
details such as the description and coordinate system of the DAS source.
We will add these later. You should also see features listed when
requesting for a segment of chromosome X:

<http://localhost:9000/das/mysource/features?segment=X:1,2000000>

You may have noticed that there are more possible data fields that may
be filled in for each feature than are included in your data file.
Whilst some of these are optional (e.g. group, target, note, link)
others are not. The DAS specification details which of the fields are
required and the appropriate content, but for now set the following
using the "fill-in" technique documented in the POD of the
Bio::Das::ProServer::SourceAdaptor::file adaptor:

-   typecategory = structural
-   type = clone
-   method = EcoRI digest
-   phase = - (features unrelated to phase)
-   score = - (features without a score)

Metadata
--------

Your DAS source now has a functioning features command. However, though
we know that it accepts chromosomes as segment IDs, client software has
no way to tell. It is therefore important to provide this information
via the sources command. Take a look at the metadata section of the
ProServer guide (in the doc

-   coordinates (human NCBI36 chromosomes)
-   title
-   description
-   maintainer
-   doc\_href (URL of more info)

`[mysource]`  
`state        = on`  
`adaptor      = file`  
`filename     = /path/to/clones.txt`  
`cols         = segment,start,end,ori,id`  
`feature_query= field0 = %segment and field2 >= %start and field1 <= %end`  
`title        = Tutorial Source`  
`description  = Some clones`  
`coordinates  = NCBI_36,Chromosome,Homo sapiens -> X:1,2000000`  
`maintainer   = user@domain.com`  
`doc_href     = `[`http://www.example.com`](http://www.example.com)

Test the output of the sources command in your browser and in the
terminal to make sure you have set all these properly.

Writing a SourceAdaptor
-----------------------

Often, you will have data in a format that is not generic or must be
manipulated in a specific manner before it is served via DAS. In these
cases, you will want to extend or create a SourceAdaptor plugin.

In its most basic form, a SourceAdaptor is a single module extending
from the Bio::Das::ProServer::SourceAdaptor package, with two methods.
Start by creating a new file with the following skeleton content:

`package Bio::Das::ProServer::SourceAdaptor::tutorial; # package names must take this form`  
`use strict;`  
`use base qw(Bio::Das::ProServer::SourceAdaptor); # modules must extend from this`  
  
`# Set metadata such as the commands supported by this source.`  
`sub init {`  
` my ($self) = @_;`  
` $self->{'capabilities'} = { 'features' => '1.0' }; # Implement the features command`  
`}`  
  
`# Gather the features annotated in a given segment of sequence.`  
`sub build_features {`  
` my ($self, $args) = @_;`  
` my $segment = $args->{'segment'}; # The query segment ID`  
` my $start   = $args->{'start'};   # The query start position (optional)`  
` my $end     = $args->{'end'};     # The query end position (optional)`  
` my @features = ();`  
` # do work...`  
` return @features;`  
`}`  
  
`1;`

Save this file as lib/Bio/Das/ProServer/SourceAdaptor/tutorial.pm. Now
try adding a new source using this adaptor, and running the server
again. Note: make sure you rebuild the ProServer installation first to
include the additional file:

`./Build`  
`eg/proserver -x -c eg/tutorial.ini`

Your source should appear in the list. The table has columns for extra
details such as the description and coordinate system of the DAS source.
We will add these later.

As an exercise in coding a SourceAdaptor, now we shall expand our
'tutorial' SourceAdaptor to serve the features from our file of clones.
To do this, the adaptor should return an array of simple hash
structures. The POD documentation for the build\_features method in
Bio::Das::ProServer::SourceAdaptor contains full details of the format
these hash structures can take. There is some flexibility here, but our
features will look like this:

`{`  
` 'start'  => $feature_start,`  
` 'end'    => $feature_end,`  
` 'id'     => $feature_id,        # A unique ID for the feature`  
` 'type'   => $feature_type,      # e.g. 'exon', 'snp'`  
` 'method' => $annotation_method, # e.g. 'similarity'`  
` 'score'  => $annotation_score,  # e.g. '96.5'`  
`}`

Expand the build\_features method of your tutorial adaptor to do the
following:

1.  . Open the file for reading
2.  . Iterate over each line
3.  . Build a DAS feature structure for features that overlap with the
    query segment
4.  . Return an array of feature structures

Test your adaptor by checking that ProServer responds appropriately to a
request for features:

` `[`http://localhost:9000/das/mysource/features?segment=X:1,2000000`](http://localhost:9000/das/mysource/features?segment=X:1,2000000)

Once you have finished, your adaptor should look something like this
(click here to show/hide):

`package Bio::Das::ProServer::SourceAdaptor::tutorial; # package names must take this form`  
`use strict;`  
`use base qw(Bio::Das::ProServer::SourceAdaptor); # modules must extend from this`

`# Set metadata such as the commands supported by this source.`  
`sub init {`  
` my ($self) = @_;`  
` $self->{'capabilities'} = { 'features' => '1.0' }; # Implement the features command`  
`}`  
  
`# Gather the features annotated in a given segment of sequence.`  
`sub build_features {`  
` my ($self, $args) = @_;`  
` my $segment = $args->{'segment'}; # The query segment ID`  
` my $start   = $args->{'start'};   # The query start position (optional)`  
` my $end     = $args->{'end'};     # The query end position (optional)`  
` my @features = ();`  
` # do work...`  
` `  
` open FH, '<', '/tmp/clones.txt' or die "Unable to open data file";`  
` while (defined (my $line = `<FH>`)) {`  
`   chomp $line;`  
`   my ($f_seg, $f_start, $f_end, $strand, $f_id) = split /\t/, $line;`  
`   `  
`   $f_seg eq $segment || next;`  
`   if ((!$start || !$end) || ($f_start <= $end && $f_end >= $start)) {`  
`     $f_id =~ s/[^=]+=//;`  
`     `  
`     my $feature = {`  
`       'id'           => $f_id,`  
`       'start'        => $f_start,`  
`       'end'          => $f_end,`  
`       'ori'          => $strand,`  
`       'method'       => 'EcoRI digest',`  
`       'score'        => '-',`  
`       'phase'        => '-',`  
`       'type'         => 'clone',`  
`       'typecategory' => 'structural',`  
`     };`  
`     `  
`     push @features, $feature;`  
`   }`  
`   `  
` }`  
` close FH;`  
` `  
` return @features;`  
`}`  
  
`1;`

You now have your DAS source up and running. Once again, fill in some of
the metadata properties (shown in the sources command) but this time you
can use different ways of doing it other than setting INI properties. -
in the init method or by implementing the relevant method in your
SourceAdaptor.

Once you have filled in these properties, start your server again. But
this time, allow the server to fork so that it is running as a daemon
process. This is done by omitting the '-x' command-line flag. Further
Tasks Register the source

Try to validate your source using the DAS Registry. Other SourceAdaptor
methods

There are several other SourceAdaptor methods that may be useful to
implement. For example, the segment\_version method makes your source
indicate the version of the segment that it is annotating. This is
useful for clients to verify that annotations are based on the same
entity. Note that not all coordinate systems have versioned entities -
for example, genomic assemblies are versioned as a whole rather than
per-entity. The known\_segments and length methods, if implemented,
allow ProServer to automatically offer the entry\_points command, and
also filter requests for unknown or out-of-range segments.

Of course, to provide this information you would need to store the
versions and lengths of all the sequences you annotate, which is worth
bearing in mind if you are planning to set up your own DAS source.
