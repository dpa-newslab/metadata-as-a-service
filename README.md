# News Agency Metadata as a Service

Transparency is essential for assessing the quality of news. Who wrote
this? Why? Whom did they talk to, what are their ethics, and what
could their conflicts of interest be? Only if you can answer those
questions you can assess the trustworthyness of a news story. 

This repository is about technicals means of distributing those bits
of information _about_ news stories - the metadata - to more readers, media
organizations, search engines and social networks. It is part of the
[Tust Project](https://thetrustproject.org/) 


## Background

News agencies face a huge legacy problem. Although better technologies
have been available for years, most of the times news agencies' stories are
stored and delivered in a capability-poor format conceived when teletype 
machines were the means of getting stories around the gloube in
the last century. A format hostile to hyperlinks, that will then be processed by
poorly standardized, slow-release-cycle legacy systems. A human
copy-and-pasting plain text is often part of this workflow, making the
automatic application of updates and corrections impossible. (More
generally, editorial content management systems tend to have the same
limitations, regardless of the use of news agency material)

This whole ecosystem is not build to easily accomodate new metadata
needed for the kind of transparency that can help make news more trustworthy.


## A new channel for Metadata

dpa is experimenting with a new channel that can deliver the missing
metadata for a story directly to the place where it can be processed,
whithout touching legacy systems. 

For example, machine-readable  metadata can be delivered directly to
the browser via a small Javascript snippet (see below). Or it can be
included by a server-side mechanism like server-side includes or
edge-side includes.  

All you need is the ID of the article. With that information, the 
metadata can be obtained from the metadata service.

### Example

Article id is

`urn:newsml:dpa.com:20090101:180923-99-82825` 

Metadata service URL can be obtained by adding the id to the 
address of our service endpoint. 

`https://metadata.dpa-prototype.de/trustproject/urn:newsml:dpa.com:20090101:180923-99-82825`


## How this could be implemented in the browser 

Below you can see part of the code for our website
[https://dpa-international.com](https://dpa-international.com). The
Trust Project markup is included via Javascript. This form of
inclusion of the metadata validates with the [schema.org Structured
Data testing
tool](https://search.google.com/structured-data/testing-tool/u/0/)

```
"use strict";

module.exports = {
  createElem: function(className, jsonld) {
    var elem = document.createElement("script");
    elem.classList = className;
    elem.type = "application/ld+json";
    elem.text = JSON.stringify(jsonld);
    document.body.appendChild(elem);
  },

  injectSite: function() {
    if (!window.enableTrust) return
    var self = this;
    $.getJSON("<metadata service for publisher>")
      .then(function(jsonld) {
        self.createElem("trust_jsonld_site", jsonld)
      })
  },

  injectArticle: function(jsonld) {
    if (!window.enableTrust) return
    var self = this;
    $.getJSON("<metadata service for article>")
      .then(function(jsonld) {
        self.createElem("trust_jsonld_site", jsonld)
      })
  }
}
```





