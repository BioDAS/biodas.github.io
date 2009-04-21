---
title: Dazzle Tutorial
permalink: wiki/Dazzle_Tutorial/
layout: wiki
---

Dazzle Tutorial Introduction
============================

This tutorial will take you through setting up Dazzle (in the Eclipse
Environment and using Tomcat as your container) as a DAS server using
the already available test sources and then it will take you through how
to write your own plugin using the this setup.

Setting up dazzle in eclipse makes it easier to debug the application if
configuration files are incorrect and makes it easy to test your das
server in an integrated environment. This step by step guide should be
usable by anyone with only a moderate knowledge of java and web
development. It describes how to set up a basic dazzle instance with the
default plugins available and test datasets included in the application.

Steps required to set up dazzle in eclipse with already available plugins
=========================================================================

Setting up dazzle in eclipse makes it easier to debug the application if
configuration files are incorrect and makes it easy to test your das
server in an integrated environment. This step by step guide should be
usable by anyone with only a moderate knowledge of java and web
development. It describes how to set up a basic dazzle instance with the
default plugins available and test datasets included in the application.

### Download container (apache tomcat 5.5.27)

Go to
[<http://tomcat.apache.org/download-55.cgi>"](http://tomcat.apache.org/download-55.cgi)
and download tomcat by clicking on a link under the core catagory -
windows installer if you are using windows or a tar.gz if you are using
linux or macOSX

### Download EasyEclipse (EasyEclipse server java version which has Web tools already or the Ganymede version).

Go to <http://www.easyeclipse.org/site/distributions/server-java.html>
and select the download for your type of operating system e.g. windows
or mac

### Set up a server configuration in eclipse

1.  right click on the servers tab at the bottom of eclipse.
2.  click new, server.
3.  browse to the tomcat 5.5.27 dir and select.
4.  next, next, ok.

### Get project from the latest source in subversion

1.  choose file, import, SVN, checkout projects from svn. (if this
    option does not exist you need to install the SVN plugin for eclipse
    [Subclipse](http://subclipse.tigris.org/)
2.  click next (create new repository location).
3.  type in "<http://www.derkholm.net/svn/repos/dazzle/>"
4.  next
5.  select the trunk directory
6.  next
7.  leave the default "check out the project using the New Project
    Wizard"
8.  click "finish"
9.  open web dir then select dynamic web project, next
10. type in the name of your project as "das"
11. click next
12. in the "Content directory:" field type in "dazzle-webapp"
13. click "finish"
14. click ok if eclipse talks about standard resources

`Your das project should now be in eclipse.`

1.  right click on the project in the eclipse explorer window and select
    "build path" then "configure build path","add jars" then open the
    project in the popup that appears and a jars dir should be visible,
    then select all the jar files, then ok.

(newer features require java 5.0 so we need to make sure the project is
configured to use java 5.0 as standard). rigt click on the project and
select properties then the J2EE tab, then select the jar files as before
to be added to the project.

1.  go to project properties again and select the java build path,
    source tab- then click "add resources" tick the resources folder
    and ok.

### Configure using dazzle.xml

now go to the dazzlecfg.xml file that you just moved into the webcontent
folder and alter the "value" next to filename for all occurances and put
a / in front e.g. change "test.embl" to "/test.embl"

### Run using eclipse

1.  Right click on the project in the window on the left and select "run
    as" then "run on server", "finish". Another window should appear
    with the url <http://localhost:8080/das/> at the top and a list of
    das sources.
2.  Alternatively:
3.  Right click on the "tomcat 5.5..." server in the servers tab in the
    bottom window of eclipse and choose start.
4.  If you navigate to <http://localhost:8080/das/> in a internet
    browser you should now see a list of das sources!!!???
5.  now type into the browser the url <http://localhost:8080/das/dsn>
6.  right click on the web page and select "view page source" this will
    open a file containing the raw xml that is being returned from your
    dazzle server.

### Deploy to a Tomcat container using Eclipse

1.  Right click on the project in the left hand window and select
    "export","web","WAR file","next", browse to a folder where you want
    to put the war file, click "save" and then "finish". You can now
    move this .war file into the webapp directory of any java compliant
    webapp container such as tomcat or resin).

Adding your own Data Source and writing your own plugin
=======================================================

Using the above setup in of Dazzle in Eclipse we are now going to write
our own Java classes to get data from our own datasource.

Background information on Dazzle and it's structure/methodology.
----------------------------------------------------------------

If you wish to get stuck into coding straight away carry on with this
page. If you wish to get an overview of the inner workings of Dazzle go
\[DazzleMethodologyAndStructure Dazzle Methodology And Structure\].

Writing the plugins (a genomic dazzle data source)
--------------------------------------------------

1.  right click on the original das project then select "new source
    folder" type in the name of your new source folder, I used "mySrc"
    as a name - click next.
2.  add a package called myplugins to this source folder and a class
    MyGFFSource

Note: to use the latest biojava code you can get it from svn and check
it out as a normal java project then run ant on the build.xml file. then
delete the biojava.jar currently set and add the new biojava.jar from
the ant-build directory of your new biojava project by adding it to the
J2EE dependencies.

`BasicData.txt file is in this format:`

`1  799614  988150      RP11-54O7`  
`1  1115132 1227290     RP5-902P8`  
`1  1419309 1506414     RP5-832C2`  
`1  1609533 1719442     RP1-283E3`  
`1  1682617 1882900     RP1-140A9`  
`1  1825391 2014198     RP11-547D24`  
`1  2062380 2242269     RP11-181G12`  
`1  2368110 2522845     RP3-395M20`  
`1  2847618 3039179     RP11-333E3`  
`1  3214521 3355092     RP4-785P20`  
`1  3391696 3538713     RP11-46F15`  
`1  3628936 3737792     RP1-286D6`  
`1  4220465 4395827     RP11-168B8`  
`1  4476687 4608114     RP1-37J18`  
`1  5189627 5308621     RP4-703E10`  
`1  5866139 6042830     RP11-49J3`  
`1  6125992 6292510     RP1-120G22`  
`1  6411973 6587996     RP11-58A11`  
`1  6661384 6810842     RP11-242F24`  
`1  7059893 7186680     RP3-438L4`  
`1  7146893 7259359     RP3-505B13`  
`1  7316045 7478083     RP4-549F15`  
`1  7641506 7843561     RP11-338N10`  
`1  7705429 7868155     RP3-467L1`  
`1  7960195 8134265     RP11-478I22`

Step 1= set up a basic features request for this gff type file (note
this not a complete gff file, which is why we can't use a ready made
plugin).

remember if you are going to set up a gff type reader and read from a
file you need to tell the servlet the filename in the dazzlecfg.xml

`<dazzle xmlns="`[`http://www.biojava.org/2000/dazzle`](http://www.biojava.org/2000/dazzle)`">`  
`  <!-- Test reference server -->`  
  
`   <datasource id="JonsPlugin" jclass="myplugins.MyDataSource">`  
`   <string name="name" value="MyFirstPlugin" />`  
`   <string name="description" value="a demo for how to write a dazzle plugin"/>`  
`   <string name="fileName" value="/BasicData.txt"/>`  
`   <string name="version" value="1.0"/>`  
`   </datasource>`  
`</dazzle>`

you will also need to make sure that eclipse restarts the server in
order for any changes in the dazzlecfg.xml to take effect. you also need
getFileName and setFileName() methods for dazzle to set the filename in
your DazzleDataSource.

here we extend the AbstractGFFFeatureSource which is itself an extension
of the AbstractDazzleDataSource which implements the DazzleDataSource
interface. In this way we are holding true to the contract that needs to
upheld to impement a DazzleDataSource plugin.

The methods in our class getFileName and setFileName allow the filename
to be specified in the dazzlecfg.xml file as above. In the public void
init(ServletContext ctx) method we allow dazzle to initialize our data
by calling this method which then calls our parseData method. In the
parse data method we read in the whole of the data file into memory (if
you are using a database you probably would initialize the connections
instead).

A utility class you will need to run this code is here:

`package myplugins;`  
  
`import java.util.ArrayList;`  
`import java.util.Iterator;`  
  
`import org.biojava.servlets.dazzle.Segment;`  
`import org.biojava.servlets.dazzle.datasource.GFFFeature;`  
  
`/**`  
` * simple class for demo purposes that returns features from given`  
`segments of a chromosome`  
` * that has probably been specified in gff type format`  
` * @author jw12`  
` *`  
` */`  
`public class Chromosome {`  
`       private String id="";`  
`       private int start=1;`  
`       private int end;`  
`       ArrayList features;`  
`       public Chromosome(String id, int end){`  
`        this.id=id;`  
`        this.end=end;`  
`        features =new ArrayList();`  
`       }`  
`       public void addFeature(GFFFeature feature){`  
`               this.features.add(feature);`  
`       }`  
`       public GFFFeature[] getFeatures(Segment seg){`  
`               System.out.println("seg in chromosome=" seg);`  
`               ArrayList featuresOnSegment=new ArrayList();`  
`               int start=seg.getStart();`  
`               int stop =seg.getStop();`  
`               //loop over arraylist of features and return a lis`  
`               Iterator it=features.iterator();`  
`            while(it.hasNext()){`  
`                       GFFFeature feature=(GFFFeature)it.next();`  
`                       int fStart=Integer.parseInt(feature.getStart());`  
`                       int fStop=Integer.parseInt(feature.getEnd());`  
`               //if between start and stop then include in returned list`  
`                       if((fStart>=start && fStart<=stop)||(fStop<=stop && fStop>=start)){`  
`                               featuresOnSegment.add(feature);`  
`                       }`  
`               }`  
`               return (GFFFeature[]) featuresOnSegment.toArray(new`  
`GFFFeature[featuresOnSegment.size()]);`  
`               //return new GFFFeature[0];`  
`       }`  
`}`

`package myplugins;`  
`import java.io.BufferedReader;`  
`import java.io.IOException;`  
`import java.io.InputStreamReader;`  
`import java.util.HashMap;`  
`import java.util.StringTokenizer;`  
  
`import javax.servlet.ServletContext;`  
  
`import org.biojava.servlets.dazzle.Segment;`  
`import org.biojava.servlets.dazzle.datasource.AbstractGFFFeatureSource;`  
`import org.biojava.servlets.dazzle.datasource.DataSourceException;`  
`import org.biojava.servlets.dazzle.datasource.GFFFeature;`  
  
`public class MyDataSource extends AbstractGFFFeatureSource {`  
  
`   String fileName;`  
`   //private  ArrayList allFeatures;`  
`   HashMap chromosomes;`  
  
  
  
`   public String getFileName() {`  
`       return fileName;`  
`   }`  
  
  
  
`   public void setFileName(String fileName) {`  
`       this.fileName = fileName;`  
`   }`  
  
  
  
`   public void init(ServletContext ctx)`  
`   throws DataSourceException`  
`   {`  
`       //data = new HashMap();`  
`       super.init(ctx);`  
`       try {`  
`           //parse the phobius result file`  
`           parseData(ctx);`  
  
  
`       } catch (Exception ex) {`  
`           throw new DataSourceException(ex, "Couldn't load data file");`  
`       }`  
`   }`  
  
  
  
`   private void parseData(ServletContext ctx) {`  
  
`       chromosomes=new HashMap();`  
`       //allFeatures=new ArrayList();`  
`       System.out.println("filename in my source=====" fileName);`  
`       BufferedReader br = new BufferedReader(new InputStreamReader(ctx.getResourceAsStream(fileName)));`  
  
`       try {`  
`           for(String line = br.readLine(); line != null; line = br.readLine()) {`  
`               System.out.println("line in file =" line);`  
`               if ( line.charAt(0) == '#')`  
`                   continue;`  
`               StringTokenizer token = new StringTokenizer(line,"\t");//split line by tabs`  
`               if (token.countTokens() < 5) {`  
`                   throw new IOException("file does not match expected format at line "   line   " it has got"    token.countTokens()   " tokens!");`  
`               }`  
`               //create a new GFFFeature for each line and add to an arraylist`  
`               String []fields=new String[5];`  
`               int i=0;`  
`               while(token.hasMoreTokens()){`  
`                   String tok=(String)token.nextElement();`  
`               fields[i]=tok;`  
`               //System.out.println("token added ok");`  
`               i  ;`  
`               }`  
`               String chromosomeId=fields[0];`  
  
`               GFFFeature feature = new GFFFeature();`  
`               feature.setOrientation(fields[3]);`  
`               feature.setType("blast");`  
`               feature.setStart(fields[1]);`  
`               feature.setEnd(fields[2]);`  
`               feature.setMethod("BLAST");`  
`               feature.setName(fields[4]);`  
`               feature.setLabel("label here");`  
`               //allFeatures.add(of);`  
`               Chromosome chr=null;`  
`               if(chromosomes.containsKey(chromosomeId)){`  
`                   chr=(Chromosome)chromosomes.get(chromosomeId);`  
  
`               }else{`  
`                   chr=new  Chromosome(chromosomeId, 100000000);`  
`                   chromosomes.put(chromosomeId,chr);`  
  
  
`               }`  
`               chr.addFeature(feature);`  
`               //of.setLink("`[`http://phobius.cgb.ki.se`](http://phobius.cgb.ki.se)`");`  
`               //of.setScore(o);`  
`               //of.setType(of.getName());`  
  
  
`           }`  
`       } catch (IOException e) {`  
`           // TODO Auto-generated catch block`  
`           e.printStackTrace();`  
`       }`  
  
`   }`  
  
`   /**`  
`    * we have overridden the super class method here as we need to.`  
`    */`  
`   public String getFeatureID(GFFFeature f) {`  
  
`       return f.getName();`  
`   }`  
  
  
`   public GFFFeature[] getFeatures(Segment seg, String[] types)`  
`           throws DataSourceException {`  
`       System.out.println("got a features request from a segement " seg);`  
`       System.out.println("segment id=" seg.getReference() " segment start=" seg.getStart() " seg stop=" seg.getStop());`  
`       //note there is also a getMax and getMin methods for the segment class in case stop is less than start???`  
  
`       //seg is something like 1:1,1000 if chromosomal`  
`       String chromosomeId=seg.getReference();`  
`       Chromosome chr=(Chromosome) chromosomes.get(chromosomeId);`  
`       GFFFeature[] features=chr.getFeatures(seg);`  
`       return features;`  
`       //return new GFFFeature[0];`  
`   }`  
  
`   public GFFFeature[] getFeatures (String reference) throws DataSourceException {`  
`       System.out.println("got features from reference" reference);`  
`       return new GFFFeature[0];`  
`   }`  
  
  
`}`

`once you have got your features cmd working you can download the dasobert project from svn and test it by creating a class like this (you can add this class to the same package as your plugins):`

`package myplugins;`  
  
`import org.biojava.dasobert.dasregistry.Das1Validator;`  
  
`public class ValidateMySource {`  
  
`   public static void main(String args[]){`  
`       String url="`[`http://localhost:8080/das/JonsPlugin/`](http://localhost:8080/das/JonsPlugin/)`";`  
`       String cmd="features";`  
`       String testcode="1:1,10000000";`  
`       Das1Validator validator=new Das1Validator();`  
  
`       boolean isValid=validator.validateFeatures(url, testcode, false);`  
`       if(isValid)System.out.println("this is valid");`  
`   }`  
  
`}`

note: you need to add the biojava project or jar and the dasobert.jar or
project to your build path before this code will compile. You can right
click and select run as application in the eclipse explorer window on
this class.

So far we have enabled Dazzle to initialize our data source (in this
case load our flat file into memory) and enable Dazzle to run a features
cmd on our data source. Next we want to add some more functionality.
Lets enable the entry\_points cmd, the entry points spec is here-
<http://www.biodas.org/wiki/DAS1.6#Entry_Points_Command>

If your server is a reference server i.e. it serves up sequence it
should also tell people what entry\_points (in this case chromosomes and
their lengths it has). to do this we can make our class implement
DazzleReferenceSource which has two methods getSequence and
getEntryPoints.

So if we add implements DazzleReferenceSource to the declaration of our
class we can then right click on the red error mark that comes up on the
side of the main panel in eclipse and select implement unimplemented
methods. This creates the empty methods mentioned above. then we can
fill them in like this:

The other method we will need to implement if we want to show people the
lengths of our chromsomes which we do is getLandmarkLength where we are
overriding a default method in the superclass AbstractDazzleDataSource.

`/** This method deals with the DAS -entry points command.`  
`    * @return a set containing the references to the entry points`  
`    */`  
`   public Set getEntryPoints() {`  
`       Set<String> s = new TreeSet<String> ();`  
`       // this example has only one feature.`  
`       // for your real data you might want to add a SQL query here.`  
  
  
`       s.add("1");`  
`       s.add("2");`  
`       s.add("3");`  
`       s.add("4");`  
`       s.add("5");`  
`       s.add("6");`  
`       s.add("7");`  
`       s.add("8");`  
`       s.add("9");`  
`       s.add("10");`  
`       s.add("11");`  
`       s.add("12");`  
`       s.add("13");`  
`       s.add("14");`  
`       s.add("15");`  
`       s.add("16");`  
`       s.add("17");`  
`       s.add("18");`  
`       s.add("19");`  
`       s.add("20");`  
`       s.add("21");`  
`       s.add("22");`  
`       s.add("X");`  
`       s.add("Y");`  
`       return s;`  
`   }`  
  
`   /**`  
`     * Return the length of the requested sequence`  
`     */`  
  
`    public int getLandmarkLength(String ref)`  
`        throws DataSourceException, NoSuchElementException`  
`    {`  
`       //set up a hashmap with our references ie chromosome ids in as keys and their lengths as values`  
`       HashMap  lengthsMap=new HashMap();`  
`       lengthsMap.put("1", "247249719");`  
`       lengthsMap.put("2", "242951149");`  
`       lengthsMap.put("3", "199501827");`  
`       lengthsMap.put("4", "191273063");`  
`       lengthsMap.put("5", "180857866");`  
`       lengthsMap.put("6", "10000000");`  
`       lengthsMap.put("7", "10000000");`  
`       lengthsMap.put("8", "10000000");`  
`       lengthsMap.put("9", "10000000");`  
`       lengthsMap.put("10", "10000000");`  
`       lengthsMap.put("11", "10000000");`  
`       lengthsMap.put("12", "10000000");`  
`       lengthsMap.put("13", "10000000");`  
`       lengthsMap.put("14", "10000000");`  
`       lengthsMap.put("15", "10000000");`  
`       lengthsMap.put("16", "10000000");`  
`       lengthsMap.put("17", "10000000");`  
`       lengthsMap.put("18", "10000000");`  
`       lengthsMap.put("19", "10000000");`  
`       lengthsMap.put("20", "10000000");`  
`       lengthsMap.put("21", "10000000");`  
`       lengthsMap.put("22", "10000000");`  
`       lengthsMap.put("X", "10000000");`  
`       lengthsMap.put("Y", "10000000");`  
`       System.out.println("ref in landmark=" ref);`  
`       int length=-1;`  
`       String lengthString=(String)lengthsMap.get(ref);`  
`       System.out.println("paring int" lengthString);`  
`       length=Integer.parseInt(lengthString);`  
`   return length;`  
`    }`  

to implement the sequence cmd which you need to do for a reference
server (but not if you are setting up an annotation server) you can
override the getSequence method like below , but obviously changing the
sequence returned for each reference:

`  public Sequence getSequence(String ref) throws NoSuchElementException,`  
`           DataSourceException {`  
`       String seq =  "AGCCCGTATATAATATGAGAGAGGCCCCGCGCG";`  
  
`       try {`  
`           Sequence dna = DNATools.createDNASequence(seq, ref);`  
`           return dna;`  
`       } catch ( IllegalSymbolException e){`  
`           throw new DataSourceException(e.getMessage());`  
`       }`  
  
`   }`

the sequence cmd will look like this:
<http://localhost:8080/das/JonsPlugin/sequence?segment=1:1,33>

Error Segments: Because we are using the GFF set of classes we don't
need to worry about creating error segments as these are handled by the
FeaturesHandler When a request for the sequence or annotations of a
named segment may fail because either: 1. the requested segment is
outside the bounds of the reference object. 2. the reference object is
not known to the server So we don't need to do any extra coding here.

Sources cmd: Now we have our server serving some information and it now
has some capabilities we need to make it known to the world which we do
in two ways: 1) create a sources cmd response to let people and machines
know more information about our data 2) Registering our data with the
DAS Registry at <http://www.dasregistry.org>

so for our source cmd: we need to edit the sources.xml file that should
be under our dazzle-webapp dir in eclipse: We need our source to look
like this:

most of the entries are self explanatory, but it's important to put in
your email address or the contact persons email address, a description
and how recent the source is.

The coordinate system you want should be listed at the bottom of this
page: <http://www.dasregistry.org/help_coordsys.jsp> We are using a
NCBI36 human genome with chromosomes as our reference sequences. so we
want to click on the link that corresponds to this source in order to
get the information we want to add to our coordinate system xml
elements. A good tip is to go to this page and find the entry for the
coordinate system and copy and paste the xml for the coordinate system
into your sources.xml document which will avoid typos and other issues
with validation: <http://www.dasregistry.org/das1/coordinatesystem>

`<?xml version='1.0' encoding='UTF-8' ?>`  
`<?xml-stylesheet type="text/xsl" href="das.xsl"?>`  
`<SOURCES>`  
`  <SOURCE   uri="JonsPlugin"`  
`       title="Our simple almost gff DAS source"`  
`       doc_href="`[`http://www.myhomepage.org`](http://www.myhomepage.org)`"`  
`       description="Test set for promoter-finding software">`  
`  <MAINTAINER   email="JoeBloggs@myemail.org" />`  
  
`  <VERSION  uri="latest" created="2009-02-25">`  
  
`   <COORDINATES uri="`[`http://www.dasregistry.org/coordsys/CS_DS40`](http://www.dasregistry.org/coordsys/CS_DS40)`" taxid="9606" version="36"`  
`           source="Chromosome"`  
`       authority="NCBI"`  
`       test_range="1:1,1000000"/>NCBI_36,Chromosome,Homo sapiens</COORDINATES>`  
  
`   <CAPABILITY type="das1:sequence" query_uri="`[`http://localhost:8080/das/JonsPlugin/sequence`](http://localhost:8080/das/JonsPlugin/sequence)`" />`  
`   <CAPABILITY type="das1:features" query_uri="`[`http://localhost:8080/das/JonsPlugin/features`](http://localhost:8080/das/JonsPlugin/features)`" />`  
`   <CAPABILITY type="das1:entry_points" query_uri="`[`http://localhost:8080/das/JonsPlugin/entry_points`](http://localhost:8080/das/JonsPlugin/entry_points)`" />`  
  
`   <CAPABILITY type="das1:stylesheet" query_uri="`[`http://localhost:8080/das/JonsPlugin/stylesheet`](http://localhost:8080/das/JonsPlugin/stylesheet)`" />`  
  
`   <PROP name="label" value="BioSapiens" />`  
`  </VERSION>`  
`  </SOURCE>`  
`</SOURCES>`

our capabilities of our server should be listed and as we are conforming
to DAS1 we put das1: in front of the commands we support. obviously
localhost:8080 will need to be replaced with your domain name when your
Dazzle server is deployed to a live server.

we can add the following lines of code to our validator to test the
sources response:

`boolean isSourcesValid=validator.validateSourcesCmd(url);`  
`       if(isSourcesValid)System.out.println("sources cmd is valid");`

We have listed in our capabilities the stylesheet cmd, but we do not
currently implement it. So lets do that now:

`stylesheet: To test this cmd: `[`http://localhost:8080/das/JonsPlugin/stylesheet`](http://localhost:8080/das/JonsPlugin/stylesheet)` We can use a stylesheet called test.style which looks like this:`

`<?xml version="1.0" standalone="yes"?>`  
  
`<DASSTYLE>`  
`  <STYLESHEET>`  
`    <CATEGORY id="default">`  
`      <TYPE id="default">`  
`        <GLYPH>`  
`     <ARROW>`  
`       <HEIGHT>10</HEIGHT>`  
`       <COLOR>yellow</COLOR>`  
`       <PARALLEL>yes</PARALLEL>`  
`     </ARROW>`  
`   </GLYPH>`  
`      </TYPE>`  
`    </CATEGORY>`  
`  </STYLESHEET>`  
`</DASSTYLE>`

`to use this test stylesheet with our dazzle source we can just make sure the file is under the dazzle-webapp directory of eclipse and then add this line to our dazzlecfg.xml file :`

`<string name="stylesheet" value="/test.style" />`

`Instructions for deploying to live server: With eclipse you can select export, as war file, then select a dir to put you .war file into. When this is done you can copy this war file to a production tomcat or resin servers webapp directory. Then to run Dazzle from tomcat you just run the following on cmd line from within the webapp dir:`

`sh ../bin/startup.sh`  
  
`and to shut down dazzle:`  
  
`sh ../bin/shutdown.sh`

`For development purposes and the purposes of this tutorial we can just continue to run our das server through eclipse as we have been.`

Setting up our server in the DAS Registry
=========================================

1.  Go to the <http://www.dasregistry.org> and select "register new"
    from the menu at the top of the page.
2.  box1: Enter and easy to remember but descriptive name in the
    nickname box
3.  box2:select the coordinate system (note you can type the first
    letter to get you close to the correct one) in our case it's N
    for NCBI. Select "NCBI\_36, Chromosome, HomoSapiens".
4.  box3: Paste in the full das path for your data source in my case
    it's
    "<http://deskpro20727.dynamic.sanger.ac.uk:8080/das/JonsPlugin>" -
    (for yours <http://deskpro20727.dynamic.sanger.ac.uk:8080> will be
    replaced by <http://www.yourdomainname.org/>
5.  box5: Put your email address
6.  box6: Put a desciption of your source and any relevant experimental
    conditions etc
7.  box7: Hold down the ctrl key and select the capabilities you have
    implimented in your source (in this tutorial we have features,
    types, source, entry\_points and sequence).
8.  box8: If you have a web page describing your project where the das
    data comes from the put the full url here
9.  box9: If you wish to add a tag/label of the types specified to it
    here
10. box10: Tick this box if you with your email to be used to let you
    know if your servers is down
11. when you're happy press save and go to next step

<!-- -->

1.  enter "1:1,33" for the test region
2.  click "register"

`You will see that our source will not validate because of the sequence cmd where we only gave a sequence of 33 bp long. If you remove the sequence cmd from the list of capabilities the source will validate.`

Looking at your server with Ensembl
===================================

`To get ensembl to read your das sources the sources.xml file needs to be configured correctly with your domain name and full path to each capability URI configured with the correct domain name.`

1.  Open a browser and go to <http://www.ensembl.org/>
2.  Select the "Human Genome" link
3.  From the left hand side menu select Karyotype
4.  Click on chromosome 1 and select "jump to location view"
5.  Click on "Add custom data to page" from the left hand menu
6.  "Attach DAS"
7.  If you have already succesfully registered your source with the
    registry you can select next and then tick your source from the
    options then available
8.  alternatively you can manually configure your source by pasting in
    the url to your das server "<http://mydomainname.ac.uk:8080/das>"
    into the box labelled "or other DAS server:"
9.  click next
10. If you sources.xml and dazzle server is working correctly ensembl
    will then take you to a page that says "The following DAS sources
    have now been attached: your das source"
11. Click "close" on the right hand side to close this configuration
    window and go back to contig view

`To verify that our dazzle server is now being read correctly by ensembl we can navigate to a region we know has a feature and see if our track is displayed along with the features:`

1.  Between the overview and the detailed veiw on the ensembl page there
    are boxes where we can specify the location of our known features
2.  In the boxes labelled "Location:" put 1 for chromosome 1 then 1 and
    1000000 for the start and end of the regions
3.  You should now see a black line towards the left of the screen
    within a track labelled "Our simple almost gff DAS source"

Writing some client code
========================

`The client code/Dasobert tutorial is here [DasobertTutorial.jsp DasobertTutorial] There are some examples of writing client code here: `[`http://www.derkholm.net/svn/repos/dasobert/trunk/doc/examples/`](http://www.derkholm.net/svn/repos/dasobert/trunk/doc/examples/)
