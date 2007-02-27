---
title: Das2Validation
permalink: wiki/Das2Validation/
layout: wiki
tags:
 - Documentation
---

<big>**DAS/2 Validation Service**</big>

About
-----

The
\[<http://crisp.cit.nih.gov/crisp/CRISP_LIB.getdoc?textkey=6712060&p_grant_num=1R01HG003040-01&p_query>=(DAS)&ticket=15416031&p\_audit\_session\_id=72191905&p\_audit\_score=100&p\_audit\_numfound=1&p\_keywords=DAS
NIH grant\] included the development of a validation service to verify
conformance of DAS/2 clients and servers with the DAS/2 specification.
This should promote consistency between different implementations and
thereby enhance interoperability of DAS/2 based systems.

Server URL
----------

<http://cgi.biodas.org:8080/>

HTTP Interface:

`  `[`http://cgi.biodas.org:8080/validate_url?url=http://example.com`](http://cgi.biodas.org:8080/validate_url?url=http://example.com)

If the validator is down, please notify the [discussion
list](http://biodas.org/mailman/listinfo/das2).

Usage Information
-----------------

This information pertains to the HTTP interface and was originally
provided by Andrew Dalke on the DAS/2 discussion list:
<http://lists.open-bio.org/pipermail/das2/2006-October/000863.html>.

### Parameter: url

Parameter "url" is required. This is the URL to validate.

    %curl 'http://cgi.biodas.org:8080/validate_url?url=http://slashdot.org/'
    <?xml version="1.0" encoding="utf-8"?>
    <DAS_VALIDATION url="http://slashdot.org/">
      <MESSAGE text="Unknown Content-Type 'text/html'." severity="error" />
    <MESSAGE text="expat: mismatched tag at byte 1794, line 29, column 3"  
    severity="fatal" />
    </DAS_VALIDATION>

### Parameter: doctype

It has an optional parameter "doctype" which is the document type to
expect

    %curl 'http://cgi.biodas.org:8080/validate_url?\
    url=http://das.biopackages.net/das/genome/human/;doctype=sources'
    <?xml version="1.0" encoding="utf-8"?>
    <DAS_VALIDATION url="http://das.biopackages.net/das/genome/human/"  
    doctype="sources" />

In that last case there were no messages.

The XML document is

    <DAS_VALIDATION url="URL-used-for-the-validation"  
    doctype="the-document-type"? >
       <MESSAGE severity="one of info, warning, error, fatal"
                text="the error message" />  *
    </DAS_VALIDATION>

A note about the doctype. If the server could not get the document then
the validation will not have a doctype even if you gave it one.

    %curl 'http://cgi.biodas.org:8080/validate_url?url=http://slashdot.org; doctype=types'
    <?xml version="1.0" encoding="utf-8"?>
    <DAS_VALIDATION url="http://slashdot.org">
      <MESSAGE text="Received Content-Type 'text/html', expected  
    'application/x-das-types+xml'." severity="fatal" />
      <MESSAGE text="expat: mismatched tag at byte 1794, line 29, column 3"  
    severity="fatal" />
    </DAS_VALIDATION>

If you tell it the wrong doctype and it gets something in XML then it
assumes the reponse is in the given doctype

    %curl 'http://cgi.biodas.org:8080/validate_url?url=http://das.biopackages.net/das/genome/human/;doctype=types'
    <?xml version="1.0" encoding="utf-8"?>
    <DAS_VALIDATION url="http://das.biopackages.net/das/genome/human/"  
    doctype="types">
      <MESSAGE text="Received Content-Type 'application/x-das-sources+xml',  
    expected 'application/x-das-types+xml'." severity="fatal" />
      <MESSAGE text="Expected element  
    '{http://biodas.org/documents/das2}TYPES' but got  
    '{http://biodas.org/documents/das2}SOURCES' at byte 41, line 3, column  
    2" severity="fatal" />
      <MESSAGE text="element &quot;SOURCES&quot; from namespace  
    &quot;http://biodas.org/documents/das2&quot; not allowed in this  
    context at byte 41, line 3, column 2" severity="error" />

If no input doctype is given then it will guess at the doctype based on
analysis of what it got from the remote server

    %curl 'http://cgi.biodas.org:8080/validate_url?url=http://das.biopackages.net/das/genome/human/'
    <?xml version="1.0" encoding="utf-8"?>
    <DAS_VALIDATION url="http://das.biopackages.net/das/genome/human/"  
    doctype="sources">
      <MESSAGE text="Assuming doctype of 'sources' based on Content-Type"  
    severity="info" />
    </DAS_VALIDATION>

This XML should be easy for anyone to parse.
