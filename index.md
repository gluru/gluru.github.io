# Kare MIND - developers documentation

Welcome to the MIND's devevelopers knowledgebase. Here you will find instructions
on how to use Kare's APIs and SDKs in order to customise your experience.


# Javascript SDK, Widget and Website integration

The simplest way to integrate the Widget with your website is by using the
[Javascript SDK](./javascript-sdk). The JS-SDK documentation explains how to
enable the Widget as well as how to configure custom operations such as callbacks and profiling.

It is also possible to use our JS-SDK in conjunction with
[Google Tag Manager (GTM) section](./google-tag-manager). GTM facilitates to
enable advanced features such as [custom events tracking](./tracking-documentation),
A/B testing and/or to load the Widget with different configurations.

To see how to add your own **callbacks** to our widget events you can check the
documentation and examples [here](./custom-callbacks).

To see code how to modify the behaviour of the widget see different code snippets and examples [here](./snippets)

# Code snippets
[Javascript SDK code snippets](./snippets)

# REST APIs

To explore our REST apis please see the following [REST APIs](http://gluru-docs.s3-website-eu-west-1.amazonaws.com/public/)

Customers can extend the platform by creating
[custom applications](./custom-apps) using the APIs.

## Importing documents and ingesting data

Customers can extend the platform by creating
[custom data fetchers](./custom-fetchers) to import data from sources that are
not yet officially supported or to implement custom features.

For security purposes, if required by your network configuration, it is also
possible to [white-list Kare's fetchers IP](./whitelisting).

# Research, publications and patents

Our research is available through our [research lab](https://www.researchgate.net/lab/KARE-Knowledgeware-Michele-Sama) hosted by Research Gate.

# System status

It's possible to receive live status updates by subscring to our [status page](https://karehq.statuspage.io/).


<script>
  window.GLR = {
    appId: 'dd940b54-b7d6-4372-9829-9287218bfb00'
  };
  (function(w, d, s){
    var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
    w.GLR = w.GLR || {};
    d.getElementsByTagName('head')[0].appendChild(j);
  })(window, document, 'script');
</script>
