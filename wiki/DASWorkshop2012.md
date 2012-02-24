---
title: DASWorkshop2012
permalink: wiki/DASWorkshop2012/
layout: wiki
---

The 2012 DAS Workshop will be held on 27-29 Feb at the EBI Hinxton
campus.

-   Information & registration:
    [registration](http://www.ebi.ac.uk/training/onsite/120227_DAS.html)

DAS Workshop 2012 Schedule
==========================

All 3 days will be held in the EBI training room.

Day 1
-----

Some knowledge/experience of PERL or Java and or Javascript would be
beneficial for the hands on training day.

### Example DAS commands can be found here as a DAS quick reference guide: [DAS examples](http://www.dasregistry.org/DASCommandExamples.jsp)

The draft schedule for Day 1 is below. Please note that session times
are a rough indication only and will vary according to progress on the
day.

| Time  | Title                                                                         | Speaker     | Resources                                                                                                                                                                                                                                                                         |
|-------|-------------------------------------------------------------------------------|-------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 9:00  | Welcome and Information                                                       | JW          |                                                                                                                                                                                                                                                                                   |
| 9:00  | DAS - Introduction/Overview                                                   | AJ          | [Introduction](http://www.biotnet.org/training-materials/introduction-das) [Technical Introduction](http://www.biotnet.org/training-materials/technical-introduction-das)                                                                                                         |
| 9:40  | The DAS Game                                                                  | LG          | [The DAS Game](http://www.biotnet.org/training-materials/das-game)                                                                                                                                                                                                                |
| 10:00 | DAS - Technical Introduction                                                  | AJ          | [Technical Introduction](http://www.biotnet.org/training-materials/technical-introduction-das)                                                                                                                                                                                    |
| 10:20 | Coffee break                                                                  |
| 10:40 | Using DAS software, an introduction to some DAS implementations               | RJ          | [Implementations Introduction](http://www.biotnet.org/training-materials/using-das-software-introduction-some-das-implementations)                                                                                                                                                |
| 11:00 | Demo: creating a DAS source from a file with EasyDAS                          | RJ          | [easyDAS tutorial](http://code.google.com/p/easydas/wiki/easyDAS_tutorial)                                                                                                                                                                                                        |
| 11:10 | Hands-on: viewing DAS sources with Ensembl OR Dasty                           | AJ/RJ       |                                                                                                                                                                                                                                                                                   |
| 11:20 | Introduction to DAS server frameworks                                         | JW          | [10 minute introduction to DAS servers](http://www.biotnet.org/training-materials/das-servers)                                                                                                                                                                                    |
| 11:30 | Hands-on programming tutorial: Creating a custom DAS source                   | AJ/JW/LG/RJ | [MyDAS](http://code.google.com/p/mydas/wiki/Tutorials) -or- [ProServer](http://www.biotnet.org/training-materials/bio-das-proserver-tutorial)                                                                                                                                     |
| 12:30 | Lunch break                                                                   |
| 13:30 | Tutorial - DAS servers continued                                              | AJ/JW/LG/RJ |                                                                                                                                                                                                                                                                                   |
| 14:50 | Introduction and hands-on: The DAS registry                                   | JW          | [DAS Registry Tutorial](http://www.biotnet.org/training-materials/short-das-registry-tutorial-basic-knowledge)                                                                                                                                                                    |
| 15:10 | Coffee Break                                                                  |
| 15:30 | Introduction to DAS client libraries                                          | LG          |                                                                                                                                                                                                                                                                                   |
| 15:40 | Hands-on programming tutorial: DAS client libraries                           | AJ/JW/LG/RJ | [Bio::Das::Lite](http://www.biotnet.org/training-materials/bio-das-lite-tutorial)-or- [JDAS](http://code.google.com/p/jdas/wiki/dasWorkshop2012) -or- [JSDAS](http://code.google.com/p/jsdas/wiki/tutorial). Each will cover sources, sequence & features using the same examples |
| 17:30 | Departure from Campus to Whittlesford Station/Holiday Inn at circa 17:30 hrs. |
| 19:00 | Dinner at the Red Lion Whittlesford 7.00 pm for 7.30 pm sit down.             |
||

Day 2 and Day 3: Developer Hackathon
------------------------------------

This year we will not have a day of talks as in previous years. Instead,
both day 2 and day 3 will be "DAS developers' days". This may include
some updates on progress since the last workshop, discussions, and a
code hackathon. The exact format is yet to be decided.

### Recent developments since the last workshop

-   DAS writeback
-   DAS search
-   Registry CRUD via a web service (get, post, put, delete) (JW)
-   Alternative content negotiation (e.g. the Registry supports JSON for
    all requests and responses) (AJ/JW)
-   Authentication/encryption in ProServer (AJ)

### Suggestions for topics on Developers Days

Please feel free to add suggestions for discussions or code sprints.

-   JSON roll out to other servers and clients? (JW)
-   Firming up of the authentication DAS standard so the registry and
    writeback are consistent (JW)
-   Add support for conditional get (If-Modified-Since - reply only if
    modified since the modified since header) or ETags? (JW)
-   High density simple features such as SNPs - vcf format or simple
    json format without method and stop? (JW)

### Possible DAS related Talks

-   DAS Registry - new architecture and capabilities (JW)
-   DAS in personal genomics (MC/JW/BG)
-   Secure DAS trial (David Martin, Jim Procter, Thomas Down)

