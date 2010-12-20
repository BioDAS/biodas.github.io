---
title: DasRegistry
permalink: wiki/DasRegistry/
layout: wiki
---

DAS Registry: Publishing and Discovery of DAS Servers
-----------------------------------------------------

The DAS registration server at
[www.dasregistry.org](http://www.dasregistry.org) provides a repository
where people can publish and share their DAS data sources (i.e., server
locations and available data) with the community.

It provides both human and programmatic interfaces to its repository of
DAS data sources. A cut-down static mirror of the main programmatic
Registry responses is available here:

-   <http://www.ebi.ac.uk/das-srv/registry/das/sources> (a mirror of
    <http://www.dasregistry.org/das/sources>)
-   <http://www.ebi.ac.uk/das-srv/registry/das/coordinatesystem> (a
    mirror of <http://www.dasregistry.org/das/coordinatesystem>)

The [DAS/1](/wiki/DAS/1 "wikilink") and [DAS/2](/wiki/DAS/2 "wikilink")
specifications do not define how to publish or discover DAS data sources
(servers). Due to the success of DAS/1 and the large number of sources
that are spread around the world it is not easy to keep track of these.

The DAS registry provdies a Web interface so users can search for
available [DAS/1](/wiki/DAS/1 "wikilink") data sources. For DAS-clients, an
XML interface is available that allows a programmatic way to retrieve
data source listings, and several DAS clients now make use of this.

For more info on this please see the
[documentation](http://www.dasregistry.org/help_scripting.jsp).

Installing local version of DAS registry
----------------------------------------

Some very rough [DAS registry installation
notes](/wiki/DAS_registry_installation_notes "wikilink") on Gregg Helt's
attempts to get a local installation of the DAS registry running, and
modifications to the registry code to support a merged DAS1/DAS2
registry.

[DAS/2](/wiki/DAS/2 "wikilink")
-------------------------

Currently, the [DasRegistry](http://www.dasregistry.org) does not
support DAS/2 services.

As a temporary fix, download the 'das2Registry.xml' file from the
[GenoViz
Project](http://genoviz.svn.sourceforge.net/viewvc/genoviz/trunk/das2_server/resources/),
add an entry, and email it back to
[GenoViz](mailto:genoviz-devel@lists.sourceforge.net) or
[BioDas](mailto:das@biodas.org). If you have access to the GenoViz SVN,
modify it directly.
