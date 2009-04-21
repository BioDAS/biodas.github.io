---
title: Dasobert Tutorial
permalink: wiki/Dasobert_Tutorial/
layout: wiki
---

Dasobert tutorial
=================

Prerequisites
-------------

This tutorial assumes you have dasobert.jar or a dasobert project
configured in eclipse. The latest source code can be imported into
eclipse using SVN from
<http://www.derkholm.net/svn/repos/dasobert/trunk> The examples here
also assume you have a das server set up as in DazzleTutorial.jsp

Set up project
--------------

Set up a java project in eclipse and call it MyDASClient. Make sure the
dasobert project or dasobert.jar is in the libraries configuration of
your new project.

Getting sequence from our das source
------------------------------------

Create a class called MyDasSequenceGetter in a package called client.

1.  Right click on the src dir in your new project
2.  new
3.  class
4.  fill in the package name and the class field and then click finish

Below is some code written using the dasobert library to get a uniprot
sequence. See if you can write our class above by adapting this code to
get our sequence from our das source. Note that there is a copy of these
examples in the examples directory of the Dasobert library as well. This
means an easy way to set up these classes is by copying and pasting the
example classes into our package using eclipse.

``  
` /*`  
`  *                    BioJava development code`  
`  *`  
`  * This code may be freely distributed and modified under the`  
`  * terms of the GNU Lesser General Public Licence.  This should`  
`  * be distributed with the code.  If you do not have a copy,`  
`  * see:`  
`  *`  
`  *      http://www.gnu.org/copyleft/lesser.html`  
`  * Copyright for this code is held jointly by the individual`  
`  * authors.  These should be listed in @author doc comments.`  
`  *`  
`  * For more information on the BioJava project and its aims,`  
`  * or to join the biojava-l mailing list, visit the home page`  
`  * at:`  
`  *`  
`  *      http://www.biojava.org/`  
`  *`  
`  * Created on 24.11.2005`  
`  * @author Andreas Prlic`  
`  *`  
`  */`  
` `  
` `  
` import org.biojava.dasobert.das.SequenceThread;`  
` import org.biojava.dasobert.dasregistry.Das1Source;`  
` import org.biojava.dasobert.eventmodel.SequenceEvent;`  
` import org.biojava.dasobert.eventmodel.SequenceListener;`  
` `  
` `  
` /** a class that demonstrates how to get a protein structure`  
`  * from a structure DAS server.`  
`  */`  
` `  
` `  
` public class GetSequence {`  
` `  
` `  
` public static void main(String[] args) {`  
`     String ac = "P50225";`  
`     if ( args.length == 1 )`  
` ac = args[0];`  
`     GetSequence s = new GetSequence();`  
`     s.showExample(ac);`  
` `  
` `  
` }`  
` `  
` `  
` public void showExample(String ac) {`  
` try {`  
` // the sequence example is very similar to the structure example.`  
` // a new Thread is created that does the DAS communication`  
` // a Listener waits for the response.`  
` `  
` `  
` // first we set some system properties`  
` `  
` `  
` // make sure we use the Xerces XML parser..`  
` System.setProperty("javax.xml.parsers.DocumentBuilderFactory",`  
` "org.apache.xerces.jaxp.DocumentBuilderFactoryImpl");`  
` System.setProperty("javax.xml.parsers.SAXParserFactory",`  
` "org.apache.xerces.jaxp.SAXParserFactoryImpl");`  
` `  
` `  
` // if you are behind a proxy, please uncomment the following lines`  
` System.setProperty("proxySet", "true");`  
` System.setProperty("proxyHost", "wwwcache.sanger.ac.uk");`  
` System.setProperty("proxyPort", "3128");`  
` `  
` `  
` // first let's create a SpiceDasSource which knows where the`  
` // DAS server is located.`  
` `  
` `  
` Das1Source dasSource = new Das1Source();`  
` `  
` `  
` dasSource.setUrl("http://www.ebi.ac.uk/das-srv/uniprot/das/aristotle/");`  
` `  
` `  
` `  
` `  
` `  
` `  
` // now we create the thread that will fetch the structure`  
` SequenceThread thread = new SequenceThread(ac, dasSource);`  
` `  
` `  
` // add a structureListener that simply prints the PDB code`  
` SequenceListener listener = new MyListener();`  
` thread.addSequenceListener(listener);`  
` `  
` `  
` // and now start the DAS request`  
` thread.start();`  
` `  
` `  
` // do an (almost) endless loop which is terminated in the StructureListener...`  
` int i = 0;`  
` while (true) {`  
` `  
` System.out.println(i   "/10th seconds have passed");`  
` i  ;`  
` Thread.sleep(100);`  
` if (i > 1000) {`  
` System.err.println("something went wrong. Perhaps a proxy problem?");`  
` System.exit(1);`  
` }`  
` `  
` }`  
` } catch (Exception e) {`  
` e.printStackTrace();`  
` }`  
` `  
` `  
` }`  
` `  
` `  
` class MyListener implements SequenceListener {`  
` `  
` `  
` /** this method is called when the Thread finishes`  
` it prints out the sequence as in Fasta format`  
` */`  
` public synchronized void newSequence(SequenceEvent event) {`  
` String accessionCode = event.getAccessionCode();`  
` String sequence = event.getSequence();`  
` `  
` System.out.println(">"   accessionCode);`  
` System.out.println(sequence);`  
` System.exit(0);`  
` }`  
` `  
` `  
` // the methods below are required by the interface but not needed here`  
` public void newObjectRequested(String accessionCode) {`  
` }`  
` `  
` `  
` public void selectionLocked(boolean flag) {`  
` }`  
` `  
` `  
` public void selectedSeqRange(int start, int end) {`  
` }`  
` `  
` `  
` public void selectedSeqPosition(int position) {`  
` }`  
` `  
` `  
` public void clearSelection() {`  
` }`  
` `  
` `  
` public void noObjectFound(String accessionCode) {`  
`             System.out.println("did not find a sequence with accession code "   accessionCode );`  
`             System.exit(0);`  
` }`  
` `  
` `  
` }`  
` `  
` `  
` }`  
` `

Getting Features
----------------

Create a new class similar to the one below and retrieve some features
from our das server and print some information out about them such as
start and end, orientation. Tip - to get the list of keys for the
features map look at the xml document from our das server that is being
read.

``  
` public class GetGenomicFeatures {`  
` `  
` `  
` //public static String DASURL = "http://www.ensembl.org/das/Homo_sapiens.NCBI36.transcript/";`  
` //public static String SEGMENT ="Y:2714896,2715740";`  
` `  
` public static final String DASURL = "http://das.sanger.ac.uk/das/ens_m37_bacmap/";`  
` public static final String SEGMENT = "12:86053507,86247661";`  
` `  
` `  
` `  
` public static void main (String[] args) {`  
` `  
` `  
` String region = SEGMENT;`  
` `  
` if ( args.length == 1 ) {`  
` region = args[0];`  
` }`  
` `  
` `  
` GetGenomicFeatures f = new GetGenomicFeatures();`  
` f.showExample(region);`  
` `  
` `  
` }`  
` `  
` `  
` public void showExample(String accessionCode) {`  
` try {`  
` `  
` `  
` // first we set some system properties`  
` `  
` `  
` // make sure we use the Xerces XML parser..`  
` System.setProperty("javax.xml.parsers.DocumentBuilderFactory","org.apache.xerces.jaxp.DocumentBuilderFactoryImpl");`  
` System.setProperty("javax.xml.parsers.SAXParserFactory","org.apache.xerces.jaxp.SAXParserFactoryImpl");`  
` `  
` `  
` // if you are behind a proxy, please uncomment the following lines`  
` System.setProperty("proxySet","true");`  
` System.setProperty("proxyHost","wwwcache.sanger.ac.uk");`  
` System.setProperty("proxyPort","3128");`  
` `  
` Das1Source dasSource = new Das1Source();`  
` `  
` dasSource.setUrl(DASURL);`  
` `  
` requestFeatures(accessionCode,dasSource);`  
` `  
` `  
` // do a loop over 10 seconds. the das sources really should respond during this time.`  
` int i = 0 ;`  
` while (true){`  
` System.out.println(i    " seconds have passed");`  
` i  ;`  
` Thread.sleep(1000);`  
` if ( i > 100) {`  
` System.err.println("We assume that das source do not take more than 10 seconds to provide a response.");`  
` System.out.println("In case you see SAX parser exceptions above - they are the result of some DAS servers not having any features in their responses and this can be ignored");`  
` System.exit(1);`  
` }`  
` }`  
` } catch (Exception e){`  
` e.printStackTrace();`  
` }`  
` }`  
` `  
` `  
` `  
` /** request the features for a singe das source.`  
` */`  
` private void requestFeatures(String accessionCode, Das1Source source) {`  
` `  
` `  
` // that is the class that listens to features`  
` FeatureListener listener = new MyListener();`  
` `  
` `  
` // now create the thread that will do the DAS requests`  
` FeatureThread thread = new FeatureThread(accessionCode, source);`  
` `  
` `  
` // and register the listener`  
` thread.addFeatureListener(listener);`  
` `  
` `  
` // launch the thread`  
` thread.start();`  
` `  
` `  
` }`  
` `  
` `  
` class MyListener`  
` implements FeatureListener{`  
` public synchronized void newFeatures(FeatureEvent e){`  
` Das1Source ds = e.getSource();`  
` Map<String,String>[] features = e.getFeatures();`  
` `  
` `  
` System.out.println("das source "   ds.getNickname()   " returned "   features.length  " features");`  
` if ( features.length>0) {`  
` `  
` `  
` `  
` `  
` `  
` `  
` `  
` }`  
` `  
` `  
` System.exit(0);`  
` }`  
` `  
` `  
` public void comeBackLater(FeatureEvent e){}`  
` }`  
` `  
` `  
` }`  
` `

Connecting to the registry
--------------------------

Create a class like the following one. Make it work and then try also
some of the other cmds you can run on the registry a guide to which can
be found here <http://www.dasregistry.org/help_scripting.jsp> Note you
may get an import missing at this stage "import
org.biojava.bio.program.das.dasalignment.DASException;" if so make sure
biojava.jar or the biojava project is in your build path.

`/*                    BioJava development code`  
` *`  
` * This code may be freely distributed and modified under the`  
` * terms of the GNU Lesser General Public Licence.  This should`  
` * be distributed with the code.  If you do not have a copy,`  
` * see:`  
` *`  
` *      `[`http://www.gnu.org/copyleft/lesser.html`](http://www.gnu.org/copyleft/lesser.html)  
` * Copyright for this code is held jointly by the individual`  
` * authors.  These should be listed in @author doc comments.`  
` *`  
` * For more information on the BioJava project and its aims,`  
` * or to join the biojava-l mailing list, visit the home page`  
` * at:`  
` *`  
` *      `[`http://www.biojava.org/`](http://www.biojava.org/)  
` *`  
` * Created on 24.11.2005`  
` * @author Andreas Prlic`  
` *`  
` */`  
  
  
`import java.net.MalformedURLException;`  
`import java.net.URL;`  
`import java.util.List;`  
`import java.util.ArrayList;`  
  
  
  
  
`import org.biojava.bio.program.das.dasalignment.DASException;`  
`import org.biojava.dasobert.das2.Das2Source;`  
`import org.biojava.dasobert.das2.DasSourceConverter;`  
`import org.biojava.dasobert.das2.io.DasSourceReaderImpl;`  
`import org.biojava.dasobert.dasregistry.Das1Source;`  
`import org.biojava.dasobert.dasregistry.DasSource;`  
  
  
`public class ContactRegistry {`  
  
`public static final String REGISTRY_LOCATION =  "`[`http://www.dasregistry.org/das1/sources`](http://www.dasregistry.org/das1/sources)`";`  
  
`    public ContactRegistry () {`  
  
`    }`  
  
`    public static void main(String[] args) {`  
`        try {`  
`            // if you are behind a proxy, please change the following lines`  
`            // for your proxy setup ...`  
`            //System.setProperty("proxySet", "true");`  
`            //System.setProperty("proxyHost", "wwwcache.sanger.ac.uk");`  
`            //System.setProperty("proxyPort", "3128");`  
  
`//          make sure we use the Xerces XML parser..`  
`            System.setProperty("javax.xml.parsers.DocumentBuilderFactory",`  
`            "org.apache.xerces.jaxp.DocumentBuilderFactoryImpl");`  
`            System.setProperty("javax.xml.parsers.SAXParserFactory",`  
`            "org.apache.xerces.jaxp.SAXParserFactoryImpl");`  
`            ContactRegistry contact = new ContactRegistry();`  
`            Das1Source[] sources = contact.getDas1Sources();`  
`            System.out.println("got "   sources.length   " DAS/1 sources:");`  
  
`            contact.displaySources(sources);`  
`        } catch ( Exception e) {`  
`            e.printStackTrace();`  
`        }`  
`    }`  
  
  
`    public Das1Source[] getDas1Sources() throws MalformedURLException, DASException{`  
  
`        DasSourceReaderImpl reader = new DasSourceReaderImpl();`  
`        URL url = new URL(REGISTRY_LOCATION);`  
  
`        DasSource[] sources = reader.readDasSource(url);`  
  
`        List das1sources = new ArrayList();`  
`        for (int i=0;i< sources.length;i  ){`  
`            DasSource ds = sources[i];`  
`             if ( ds instanceof Das1Source){`  
`                das1sources.add((Das1Source)ds);`  
`            } else if ( ds instanceof Das2Source){`  
`                Das2Source d2s = (Das2Source)ds;`  
`                if (d2s.hasDas1Capabilities()){`  
`                    Das1Source d1s = DasSourceConverter.toDas1Source(d2s);`  
`                    das1sources.add(d1s);`  
`                }`  
`            }`  
`        }`  
`        return (Das1Source[])das1sources.toArray(new Das1Source[das1sources.size()]);`  
  
`    }`  
  
`    private void displaySources(DasSource[] sources){`  
`        for (int i=0;i<2;i  ) {`  
`            DasSource ds = sources[i];`  
`            System.out.println(ds);`  
`            String[] caps = ds.getCapabilities();`  
`            for (int c=0;c<caps.length;c  ){`  
`                String cap = caps[c];`  
`                System.out.println(cap);`  
`            }`  
`        }`  
`    }`  
  
`}`

### Suggested Possible Answers

Get Sequence:

`package client;`  
  
`import org.biojava.dasobert.das.SequenceThread;`  
`import org.biojava.dasobert.dasregistry.Das1Source;`  
`import org.biojava.dasobert.eventmodel.SequenceEvent;`  
`import org.biojava.dasobert.eventmodel.SequenceListener;`  
  
`public class MyDasSequenceGetter {`  
`   public static void main(String[] args) {`  
`       String ac = "1:1,32";`  
`       if ( args.length == 1 )`  
`       ac = args[0];`  
`       MyDasSequenceGetter s = new MyDasSequenceGetter();`  
`       s.showExample(ac);`  
  
`   }`  
  
`   public void showExample(String ac) {`  
`       try {`  
`           // the sequence example is very similar to the structure example.`  
`           // a new Thread is created that does the DAS communication`  
`           // a Listener waits for the response.`  
  
`           // first we set some system properties`  
  
`           // make sure we use the Xerces XML parser..`  
`           System.setProperty("javax.xml.parsers.DocumentBuilderFactory",`  
`                   "org.apache.xerces.jaxp.DocumentBuilderFactoryImpl");`  
`           System.setProperty("javax.xml.parsers.SAXParserFactory",`  
`                   "org.apache.xerces.jaxp.SAXParserFactoryImpl");`  
  
`           // if you are behind a proxy, please uncomment the following lines`  
`//         System.setProperty("proxySet", "true");`  
`//         System.setProperty("proxyHost", "wwwcache.sanger.ac.uk");`  
`//         System.setProperty("proxyPort", "3128");`  
  
`           // first let's create a SpiceDasSource which knows where the`  
`           // DAS server is located.`  
  
`           Das1Source dasSource = new Das1Source();`  
  
`           dasSource.setUrl("`[`http://localhost:8080/das/JonsPlugin/`](http://localhost:8080/das/JonsPlugin/)`");`  
  
  
  
`           // now we create the thread that will fetch the structure`  
`           SequenceThread thread = new SequenceThread(ac, dasSource);`  
  
`           // add a structureListener that simply prints the PDB code`  
`           SequenceListener listener = new MyListener();`  
`           thread.addSequenceListener(listener);`  
  
`           // and now start the DAS request`  
`           thread.start();`  
  
`           // do an (almost) endless loop which is terminated in the StructureListener...`  
`           int i = 0;`  
`           while (true) {`  
  
`               System.out.println(i   "/10th seconds have passed");`  
`               i  ;`  
`               Thread.sleep(100);`  
`               if (i > 1000) {`  
`                   System.err.println("something went wrong. Perhaps a proxy problem?");`  
`                   System.exit(1);`  
`               }`  
  
`           }`  
`       } catch (Exception e) {`  
`           e.printStackTrace();`  
`       }`  
  
`   }`  
  
`   class MyListener implements SequenceListener {`  
  
`       /** this method is called when the Thread finishes`  
`        it prints out the sequence as in Fasta format`  
`        */`  
`       public synchronized void newSequence(SequenceEvent event) {`  
`           String accessionCode = event.getAccessionCode();`  
`           String sequence = event.getSequence();`  
  
`           System.out.println(">"   accessionCode);`  
`           System.out.println(sequence);`  
`           System.exit(0);`  
`       }`  
  
`       // the methods below are required by the interface but not needed here`  
`       public void newObjectRequested(String accessionCode) {`  
`       }`  
  
`       public void selectionLocked(boolean flag) {`  
`       }`  
  
`       public void selectedSeqRange(int start, int end) {`  
`       }`  
  
`       public void selectedSeqPosition(int position) {`  
`       }`  
  
`       public void clearSelection() {`  
`       }`  
  
`       public void noObjectFound(String accessionCode) {`  
`            System.out.println("did not find a sequence with accession code "   accessionCode );`  
`            System.exit(0);`  
`       }`  
  
  
`   }`  
  
`}`

Get Features:

`package client;`  
  
`import java.util.Map;`  
  
`import org.biojava.dasobert.das.FeatureThread;`  
`import org.biojava.dasobert.dasregistry.Das1Source;`  
`import org.biojava.dasobert.eventmodel.FeatureEvent;`  
`import org.biojava.dasobert.eventmodel.FeatureListener;`  
`import org.biojava.dasobert.feature.FeatureTrack;`  
`import org.biojava.dasobert.feature.FeatureTrackConverter;`  
  
`public class GetGenomicFeatures {`  
  
`   //public static String DASURL = "`[`http://www.ensembl.org/das/Homo_sapiens.NCBI36.transcript/`](http://www.ensembl.org/das/Homo_sapiens.NCBI36.transcript/)`";`  
`   //public static String SEGMENT ="Y:2714896,2715740";`  
  
`   //public static final String DASURL = "`[`http://das.sanger.ac.uk/das/ens_m37_bacmap/`](http://das.sanger.ac.uk/das/ens_m37_bacmap/)`";`  
`   public static final String DASURL ="`[`http://localhost:8080/das/JonsPlugin/`](http://localhost:8080/das/JonsPlugin/)`";`  
  
`   public static final String SEGMENT = "1:10000,10000000";`  
  
`   public static void main (String[] args) {`  
  
`       String region = SEGMENT;`  
  
`       if ( args.length == 1 ) {`  
`           region = args[0];`  
`       }`  
  
`       GetGenomicFeatures f = new GetGenomicFeatures();`  
`       f.showExample(region);`  
  
  
  
`   }`  
  
`   public void showExample(String accessionCode) {`  
`       try {`  
  
`           // first we set some system properties`  
  
`           // make sure we use the Xerces XML parser..`  
`           System.setProperty("javax.xml.parsers.DocumentBuilderFactory","org.apache.xerces.jaxp.DocumentBuilderFactoryImpl");`  
`           System.setProperty("javax.xml.parsers.SAXParserFactory","org.apache.xerces.jaxp.SAXParserFactoryImpl");`  
  
`           // if you are behind a proxy, please uncomment the following lines`  
`           System.setProperty("proxySet","true");`  
`           System.setProperty("proxyHost","wwwcache.sanger.ac.uk");`  
`           System.setProperty("proxyPort","3128");`  
  
  
  
`           Das1Source dasSource = new Das1Source();`  
  
`           dasSource.setUrl(DASURL);`  
  
  
`           requestFeatures(accessionCode,dasSource);`  
  
  
`           // do a loop over 10 seconds. the das sources really should respond during this time.`  
`           int i = 0 ;`  
`           while (true){`  
`               System.out.println(i    " seconds have passed");`  
`               i  ;`  
`               Thread.sleep(1000);`  
`               if ( i > 100) {`  
`                   System.err.println("We assume that das source do not take more than 10 seconds to provide a response.");`  
`                   System.out.println("In case you see SAX parser exceptions above - they are the result of some DAS servers not having any features in their responses and this can be ignored");`  
`                   System.exit(1);`  
`               }`  
`           }`  
`       } catch (Exception e){`  
`           e.printStackTrace();`  
`       }`  
`   }`  
  
  
  
  
  
  
  
  
  
`   /** request the features for a singe das source.`  
`    */`  
`   private void requestFeatures(String accessionCode, Das1Source source) {`  
  
`       // that is the class that listens to features`  
`       FeatureListener listener = new MyListener();`  
  
`       // now create the thread that will do the DAS requests`  
`       FeatureThread thread = new FeatureThread(accessionCode, source);`  
  
`       // and register the listener`  
`       thread.addFeatureListener(listener);`  
  
`       // launch the thread`  
`       thread.start();`  
  
`   }`  
  
`   class MyListener`  
`   implements FeatureListener{`  
`       public synchronized void newFeatures(FeatureEvent e){`  
`           Das1Source ds = e.getSource();`  
`           Map[] features = e.getFeatures();`  
  
  
`           System.out.println("das source "   ds.getNickname()   " returned "   features.length  " features");`  
`           if ( features.length>0) {`  
  
`               for(int i=0; i<features.length;i  ){`  
`                   String name=features[i].get("id");`  
`                   String start=features[i].get("START");`  
`                   String stop=features[i].get("END");`  
`                   String orientation=features[i].get("ORIENTATION");`  
`                   System.out.println(name  " "  start " " stop " " orientation);`  
`               }`  
  
  
  
  
`           }`  
  
`           System.exit(0);`  
`       }`  
  
`       public void comeBackLater(FeatureEvent e){}`  
  
`   }`  
  
`}`

Connect to the Registry:

`import java.net.MalformedURLException;`  
  
`import org.biojava.bio.program.das.dasalignment.DASException;`  
`import org.biojava.dasobert.dasregistry.Das1Source;`  
`import org.biojava.dasobert.dasregistry.DasCoordinateSystem;`  
`import org.biojava.dasobert.dasregistry.DasSource;`  
  
  
`public class DASWalker {`  
  
`   private Das1Source specificSource=null;`  
  
  
`   public static void main (String[] args) {`  
  
`       //want to connect to the registry list all sources then get a source and some associated info`  
`       //then walk along the source and get the data back`  
  
`        System.setProperty("javax.xml.parsers.DocumentBuilderFactory",`  
`         "org.apache.xerces.jaxp.DocumentBuilderFactoryImpl");`  
`         System.setProperty("javax.xml.parsers.SAXParserFactory",`  
`         "org.apache.xerces.jaxp.SAXParserFactoryImpl");`  
`         DASWalker walker=new DASWalker();`  
`         Das1Source []sources=walker.getAllSources();`  
`         walker.displaySources(sources);`  
  
  
  
  
`   }`  
  
`   public DASWalker(){`  
  
`   }`  
  
`   public Das1Source[] getAllSources(){`  
`       ContactRegistry contact = new ContactRegistry();`  
`        Das1Source[] sources=null;`  
`       try {`  
`           sources = contact.getDas1Sources();`  
`       } catch (MalformedURLException e) {`  
`           // TODO Auto-generated catch block`  
`           e.printStackTrace();`  
`       } catch (DASException e) {`  
`           // TODO Auto-generated catch block`  
`           e.printStackTrace();`  
`       }`  
  
`        System.out.println("got "   sources.length   " DAS/1 sources:");`  
  
  
  
`        return sources;`  
`   }`  
  
`   public void displaySources(Das1Source[] sources){`  
`       for (int i=0;i<sources.length;i  ) {`  
`            DasSource ds = sources[i];`  
`            System.out.println(ds);`  
  
`            String[] caps = ds.getCapabilities();`  
`            for (int c=0;c<caps.length;c  ){`  
`                String cap = caps[c];`  
`                System.out.println("capability=" cap);`  
  
`            }`  
`            DasCoordinateSystem [] coordinateSystems = ds.getCoordinateSystem();`  
`            for (int c=0;c<coordinateSystems.length;c  ){`  
`                DasCoordinateSystem coordSystem = coordinateSystems[c];`  
`                System.out.println("coordinate system name=" coordSystem.toString() " unique id=" coordSystem.getUniqueId());`  
`            }`  
`        }`  
  
  
`   }`  
  
`}`
