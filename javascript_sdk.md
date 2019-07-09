# Kare MIND Widget

This documentation explains how to setup and present Kare MIND widget on your website.

It can be triggered with a button launcher (default), or launching via calling the javascript API.

## Installation

Before you can use the widget you will need to add the following snippet to the website you wish to track. This will enabled the widget with button launcher by default. See [Advanced Configuration](#advanced-configuration) to change this behaviour.

**Please use the snippet from the settings page in your console. The appId here is just an example. Also, the environment of the widget source is location dependent. This example uses the `us` environment but yours may differ.**


```
<script>
window.GLR = {
  appId: 'APPID-FROM-CONSOLE'
};

(function(w, d, s){
  var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://gluru-widget.eu.gluru.co/latest.js';
  w.GLR = w.GLR || {};
  d.getElementsByTagName('head')[0].appendChild(j);
})(window, document, 'script');
</script>
```

# Advanced configuration

Configuration options to pass to `window.GLR` before widget setup.

|  Property    |   Type    |  Description |
|--------------|-----------|--------------|
| appId        |   string  | Your client key. Required. |
| showLauncher |   bool    | Show or hide the button launcher. Defaults to `true`. Useful if you want to use your own custom button to launch the widget. Optional. |
| isFullscreen | bool      | Loads the widget in fullscreen. This removes navigation so the end-user cannot close the widget. This is ideal for mobile app webviews.|
| userId          |   string  | Set the user id in the config. This will get sent down on initialization can will appear in the console. Optional.|
| customHeaders   |   Object  | Send down custom headers to the backend so add metadata to your customers’ conversations. Only custom headers that start with “kare-meta-” will be accepted. Any headers that do not follow this pattern will be ignored and will not be sent. Optional. |


## Fullscreen

In order to make the widget fullscreen on a particular page you can either add the `isFullscreen` property to the `GLR` object. For example:

```
window.GLR = {
  appId: 'APPID-FROM-CONSOLE',
  isFullscreen: true,
  userId: 'my-unique-user-id',
  customHeaders: {
    'kare-meta-my-custom-header': 'my-custom-value',
    'kare-meta-custom-header-two': 123
  }
};
```

or you can add the `isFullscreen` querystring to the parent URL. For example:

```
https://www.page-where-widget-is-loaded.com?isFullscreen
```

# API

The Javascript SDK allows web developers to control the widget programmatically. The API can be accessed via the `window.kare`.

| Property             |  Type   | Value            |
|----------------------|---------|------------------|
| open()  | method  | Opens the widget bottom right and starts a session |  
| close()  | method  | Closes the widget window completely |
| showLauncher()  | method  | Show the widget launcher |
| hideLauncher()  | method  | Hide the widget launcher |
| onClose(callback)  | method  | Calls the callback that has been passed as an argument when the widget is closed. We send the conversation ID as a parameter to the callback method. Example: `kare.onClose(function(event){console.log('my custom close callback')})`. This will log ‘my custom close callback’ |
| onOpen(callback)  | method  | Open event, triggered with a callbackEvent when the widget is opened by a user. |
| onEscalate(callback)  | method  | Escalate event, triggered is the user clicks on any escalation button. |


## Launching the widget programmatically with Javascript.

```
window.kare.open();
```

```
window.kare.close();
```

```
window.kare.showLauncher();
```

```
window.kare.hideLauncher();
```

## Setting event handlers programmatically with Javascript.

```
window.kare.onClose(function(event){
  console.log('Widget was closed.', event.conversationId, event.createdAt)
});
```

```
window.kare.onOpen(function(event){
  console.log('Widget was opened.', event.conversationId, event.createdAt)
});
```

```
window.kare.onEscalate(function(event){
  console.log('Widget escaltion was triggered.', event.conversationId, event.createdAt)
});
```

### Callbacks

All callbacks methods will be invoked with a single event parameters which looks like the following.

```
callbackEvent {
    “conversationId”: “<id>”, // the conversation ID assigned by the server
    “createdAt”: “<time_stamp>” // event timestamp (assigned by the Javascript SDK)
}
```

## Inside a web page

The API can also be used inside a webpage on html element events.

```
<button onclick="window.kare.open();">Open at side of page</button>
<button onclick="window.kare.open({ query: 'How can I track my order?' });">Open with query</button>
<button onclick="window.kare.close();">Close the widget</button>
```



