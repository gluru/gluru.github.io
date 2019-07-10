# Welcome to Kare MIND

## Tracking Documentation


## Javascript SDK, Widget and Website integration

The simplest way to integrate the Widget with your website is by using the[Javascript SDK](./javascript-sdk). The Javascript SDK documentation explains how to enable the Widget as well as how to configure custom operations such as callbacks and profiling. 

It is also possible to use our Javascript SDK in conjunction with [Google Tag Manager](./tracking-documentation). GTM can be used to for advanced events tracking as well as to do A/B testing and/or to load the Widget with different configurations.


## REST APIs

To explore our REST apis please see the following [REST APIs](http://gluru-docs.s3-website-eu-west-1.amazonaws.com/public/)

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

## Custom callbacks

To see how to add your own callbacks to our widget events you can check the documentation and examples [here](./custom-callbacks).

## Google Tag Manager integration

If you need some help to add our widget using GTM, you can check our [Google Tag Manager section](./google-tag-manager)