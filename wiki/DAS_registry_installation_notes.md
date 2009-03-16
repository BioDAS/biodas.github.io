---
title: DAS registry installation notes
permalink: wiki/DAS_registry_installation_notes/
layout: wiki
---

**Notes on getting Sanger DAS registry running locally (on OSX)**

**Java code:**

    BioJava: checked out head of trunk (svn://code.open-bio.org/biojava/biojava-live/trunk)
    Dasobert: checked out head of trunk (http://www.derkholm.net/svn/repos/dasobert/trunk)
    DasRegistry: checked out head of openid branch, as per disccusion with Jonathon 
                  (http://www.derkholm.net/svn/repos/dasregistry/branches/openid)
        To build dasregistry, started new NetBeans project as "Web Application with Existing Sources"
                    rather than free-form project, because having problems with checked in build.xml file
                    this way, NetBeans creates a new build file (nbbuild.xml)

    JARs required but not present in repository:
    mysql-connector-java-5.1.7-bin.jar (MySQL Connector/J JDBC driver)
    axis.jar (Axis 1.4)
    jaxrpc.jar
    biojava.jar (head of biojava trunk)
    dasobert.jar (head of dasobert trunk)
    cewolf-1.0.jar
    jfreechart-1.0.10.jar
    commons-logging-1.0.4.jar (Apache commons)
    mail.jar (javamail-1.41)
    ehcache-1.6.0-beta3.jar
    ols-client.jar (Ontology Lookup Service)
    java-openid-sxip-0.9.4.jar
    commons-codec-1.3.jar (Apache commons)
    commons-httpclient-3.0.1.jar (Apache commons)
    commons-logging-1.03.jar (Apache commons)
    htmlparser.jar (from openid-sxip)
    openxri-client.jar (from openid-sxip)
    openxri-syntax.jar (from openid-sxip)
    log4j-1.2.15.jar (Apache log4j)
    dom3-xercesImpl.jar (from openid-sxip endorsed libs)
    dom3-xml-apis.jar (from openid-sxip endorsed libs)
    xalan-2.6.0.jar (from openid-sxip endorsed libs)
    commons-fileupload-1.2.1.jar (Apache commons)
    jcommon-1.0.13.jar (from jfreechart)

Code changes required for registry compilation:

Couldn't find ontology lookup jar that corresponded with naming in head
of registry oid branch: The only compilation problem I had was with the
DasRegistryOntologyLookup class, which depends on classes in the
uk.ac.ebi.www.ontology\_lookup.OntologyQuery package. Which I couldn't
locate. The closest I could find was the Ontology Lookup Service code at
<http://www.ebi.ac.uk/ontology-lookup/>. As near as I can tell this has
the exact same classes as uk.ac.ebi.www.ontology\_lookup.OntologyQuery,
except they're in a different package: uk.ac.ebi.ook.web.services. So
I'm guessing there was just a package refactoring at some point and the
registry is using either a newer or older version of the ontology-lookup
code than what is available at the OLS site. Changed the imports in
DasRegistryOntologyLookup to use the package names in the OLS jar and
now everything compiles. Should check with Jonathon and see if where to
find ontology\_lookup.OntologyQuery package, and whether I should swap
in.

    Changes required to avoid runtime errors:
        das_registry
            org.biojava.services.das.dao.DasSourceManager.getCapabilities() method
            had to add "sources" column to SELECT statement, otherwise get "column 'sources' not found" errors when accessing ResultSet

        Tomcat / webapp setup:
        JNDI stuff in Tomcat/webapp couldn't find mysql JDBC drivers even when they
            were added as project library jars, and in several places under Tomcat $CATALINA_HOME
        had to add a RESOURCE element to context.xml in project Configuration Files:
           <Resource name="jdbc/mysql" auth="Container" type="javax.sql.DataSource"
                   maxActive="8" maxIdle="4" maxWait="10000"
                   username="javauser" password="javadude" driverClassName="com.mysql.jdbc.Driver"
                   url="jdbc:mysql://localhost:3306/dasregistry01?autoReconnect=true"/>
          (see ~/release/install.txt for Andreas' recommendation on a slightly different Resource declaration)

        Servlet package naming in WebContent/WEB-INF/web.xml is out of sync with source code, so fixed in web.xml:
        (all are just changing "servlet" to "servlets" in servlet-class element class names
        org.biojava.services.das.servlet.Das2RegistryServlet --> org.biojava.services.das.servlets.Das2RegistryServlet
        org.biojava.services.das.servlet.CoordSysServlet --> org.biojava.services.das.servlets.CoordSysServlet
        org.biojava.services.das.servlet.FileUploadServlet --> org.biojava.services.das.servlets.FileUploadServlet
        org.biojava.services.das.servlet.DasSourceActionServlet --> org.biojava.services.das.servlets.DasSourceActionServlet
        org.biojava.services.das.servlet.ProjectIconServlet --> org.biojava.services.das.servlets.ProjectIconServlet
        org.biojava.services.das.servlet.ProjectActionServlet --> org.biojava.services.das.servlets.ProjectActionServlet

WARNING!! Xerces parser problems
(org.apache.xerces.jaxp.DocumentBuilderFactoryImpl not found) conflicts
between JDK1.5 internal Xerces (crimson?) and explicit setting of
implementations in
org.biojava.services.das.registry.RegistryConfiguration (also explicitly
set often in dasobert examples, Das1Validator, some .jsp files, and
tests) Problem is that internal Xerces is auto-overriding any external
Xerces (in my case from dom3-xercesImpl.jar) in classpath but with
different package naming, which gives errors like:
javax.xml.parsers.FactoryConfigurationError: Provider
org.apache.xerces.jaxp.DocumentBuilderFactoryImpl not found See
<http://www.mbaworld.com/docs/class-loader-howto.html> , section on XML
parser class loading

`   and `[`http://java.sun.com/j2se/1.5.0/docs/guide/standards/index.html`](http://java.sun.com/j2se/1.5.0/docs/guide/standards/index.html)` for endorsed libs mechanism for overriding internal JDK/JRE classes`

Unfortunately can't just use endorsed libs in a straightforward ways
because there is also a problem with NetBeans using anything other than
the JDK1.5 internal lib, which it seems to interpret as having Xalan in
the classpath, which causes NetBeans to refuse to run at all:
<http://wiki.netbeans.org/FaqXalanOnCPmplementations> Tried several ways
of using endorsed libs mechanism in combination with using different JDK
to run NetBeans (to avoid Xalan problem mentioned above), but never
could get the combination to stick on NetBeans6.5 under OSX -- possibly
the problem is in specifying the java.endorsed.dirs system property?
SOLUTION: for now, commented out explicit setting of factory in
RegistryConfiguration NEED TO EITHER COME UP WITH ALTERNATIVE, OR ALSO
FIX IN OTHER CLASSES

**Registry MySQL database:** In general, preface everything on
command-line with sudo in order to admin stuff... Alternatively, for
mysql command line login as root:

`       mysql -u root -p`

Make sure MySQL is installed

Starting up MySQL daemon (as per below):

`   sudo /usr/local/mysql/bin/mysqld_safe `[`asks` `for`
`password`](asks_for_password "wikilink")  
`   [Cnt-Z to stop process]`  
`   bg `[`to` `restart` `process` `in`
`background`](to_restart_process_in_background "wikilink")

Set password for root, admin, to standard internal via command-line:

`   sudo mysqladmin -u root password ********`

Create das2registry01 database:

`   sudo /usr/local/mysql/bin/mysqladmin create dasregistry01`

`Populated database from registry SQL file that AndyJ sent me`  
`    (which I think is a mysqldump of the current registry database,`  
`         file is at ~/projects/external/das/das_registry_oid_branch/manually_added_db/reg.sql)         `

sudo mysql -p dasregistry01 &lt; reg.sql

Added user: "javauser", password: "javadude" via mysql client:

`   -> grant all privileges`  
`   ->  on *.* to 'javauser'@'localhost'`  
`   ->  identified by 'javadude'`  
`   ->  with grant option;`

first had to set root/admin password, otherwise got error:

`  "Access denied for user ''@'localhost' (using password: NO)"`

see discussion at ??? on problem with using MySQL JDBC drivers without
password

To see users:

`  select host, user, password from mysql.user`

Use DbVisualizer application to browse database:

`   current connection alias: "local MySQL"`  
`   database URL: jdbc:mysql://localhost:3306/`  
`        (or optionally to see _just_ dasregistry01 database:`  
`               jdbc:mysql://localhost:3306/dasregistry01 )`  
`   userid: root (or optionally "javauser")`  
`   password:  ****** (or optionally "javadude")`

**Older installation notes in code repository** Only after figuring out
all of the above did I find Andreas' notes from Nov 2005 on local
installation of the DAS registry, in the code repository at

`   $CHECKOUT_ROOT/release/install.txt`

Also the SQL for creating an empty database registry:

`   $CHECKOUT_ROOT/release/dasregistry.sql`

And the (old and out of date) war file for easy distribution

`   $CHECKOUT_ROOT/release/dasregistry.war`

WARNING!!! The installation notes, SQL file, and .war are out of date,

`   at least for the openid branch --`  
`        for example:`  
`           the SQL does not include creation of "sources" and "interaction" columns in the "registry" tab`  
`           the .war does not include _anything_ from Dasobert`

There are additional jars in $CHECKOUT\_ROOT/release/version2.0/ that
date from July 2006

`       biojavar.jar, dasobert.jar, dasregistry.jar`  
`       These are closer to current code base, for example there are Das2Source etc. classes which were not present in Nov2005 war`  
`       But still out of date -- for instance nothing on molecular interactions in dasobert jar`

THIS STUFF NEEDS UPDATING!!! STILL NEED TO DO: the installation notes
suggest putting the RESOURCE configuration for jdbc/mysql in the
$TOMCAT/conf/server.xml config file instead of in the webapp context.xml
file as I did above

**Remaining Issues** Still getting warning messages about log4j from
Tomcat when registry webapp is deployed: log4j:WARN No appenders could
be found for logger
(org.springframework.context.support.ClassPathXmlApplicationContext).
log4j:WARN Please initialize the log4j system properly.

Problems with das.xsl stylesheet

`    Browser is having trouble retrieving the das.xsl stylesheet`  
`    Because of the way the webapp is configured (mapping das1/*) and the way the stylesheet is specified in the sources XML (just as "das.xsl"), the browser does a GET on [rootpath]/das1/das.xsl.  The server, rather than directly returning the das.xsl file for this request, hands off responsibility to the Das2RegistryServlet.  Which retrieves the file as a resource stream from the servlet context and then streams it back as content of the HTTP response.  But resource-not-found exceptions are getting thrown.  Looks like problem is that at least some combinations of JVM/TomCat really want a "/" prefixed to the file name in arg to the ServletContext.getResourceAsStream() call (see `[`http://www.xyzws.com/servletfaq/how-to-use-servletcontextgetresourceasstream/18`](http://www.xyzws.com/servletfaq/how-to-use-servletcontextgetresourceasstream/18)`)`

SOLUTION: added conditional so that if "das.xsl" doesn't work for
resource retrieval, add a "/" prefix and try again.

Also repository is missing a bunch of css stylesheet files, referred to
in the das.xsl (and therefore like das.xsl, they are triggering attempts
to load as resources in Das2RegistryServlet calls (in response to
browser GETs).

`   /css/dazzle.css`  
`   /css/content.css`  
`   /css/content.css`  
`   /css/printer-styles.css`  
`   /css/screen-styles.css`

TEMPORARY SOLUTION: turned off adding of xml-stylesheet clause to
sources XML response in ServletResponseWriter (made adding it a boolean
option) NEED TO FIX: once css is checked into repository, can flip
boolean to turn xml-stylesheet back on)

Now starting to modify to support DAS2 "segments", "types", "features"
capabilities (and extend "sources" support)

Added columns to registry database for das2:segments, das2:types,
das2:features mysql -u root -p

` > use dasregistry01;`  
` > alter table registry add das2_segments bit;`  
` > alter table registry add das2_types bit;`  
` > alter table registry add das2_features bit;`

org.biojava.dasobert.das.Capabilities

`     added constants/enums for DAS2 segments, types, features capabilities`

org.biojava.services.das.dao.DasSourceManager

`     changed how get_capabilities() database query is constructed, so it is based on capabilities specified in org.biojava.dasobert.das.Capabilities constants, rather than having to be manually synchronized with Capabilities.  So addition of DAS2 capabilities constants to Capabilities is automatically reflected in DasSourceManager.  This also fixes previous runtime error problem with "sources" absence in database query, but in a more general way than original fix mentioned above.`

Major problem going forward is that the registry database doesn't store
capability URLs, but rather a URL for the versioned source and a set of
BIT/boolean columns for whether a versioned source supports a particular
capability. As near as I can tell the query\_uri of the capability is
then composed from the versioned source URI and the column name
corresponding to a capability.

It looks like there was work done by Andreas on a revised registry
database schema that supported decoupling of capability URIs from
versioned source URIs via source, capability, and source2capability
tables. Is there an instance of this database still around?
