# Kare MIND Widget

This documentation explains how to setup and present Kare MIND widget on your website.

It can be triggered with a button launcher (default), or launching via calling the javascript API.

## Installation

Before you can use the widget you will need to add the following snippet to the website you wish to track. This will enabled the docked widget with button launcher by default. See [Advanced Configuration](#advanced-configuration) to change this behaviour.

**Please use the snippet from the settings page in your console. The appId here is just an example. Also, the environment of the widget source is location dependent. This example uses the `us` environment but yours may differ.**


```
<script>
window.GLR = {
  appId: 'APPID-FROM-CONSOLE',
  showLauncher: false
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
| key          |   string  | Your client key. Required. |
| showLauncher |   bool    | Show or hide the button launcher. Defaults to `true`. Useful if you want to use your own custom button to launch the widget. |
| isFullscreen | bool      | Loads the widget in fullscreen. This removes navigation so the end-user cannot close the widget. This is ideal for mobile app webviews.|

## Fullscreen

In order to make the widget fullscreen on a particular page you can either add the `isFullscreen` property to the `GLR` object. For example:

```
window.GLR = {
  appId: 'APPID-FROM-CONSOLE',
  isFullscreen: true
};
```

or you can add the `isFullscreen` querystring to the parent URL. For example:

```
https://www.page-where-widget-is-loaded.com?isFullscreen
```

# Recipes

The Javascript SDK allows web developers to control the widget programmatically.

## Launching Programmatically with Javascript

The widget can be launched in all variants through the Javascript API, the API can be accessed via the `window.kare`.

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

### Inside a web page

```
<button onclick="window.kare.open();">Open at side of page</button>
<button onclick="window.kare.open({ query: 'How can I track my order?' });">Open with query</button>
<button onclick="window.kare.close();">Close the widget</button>
```

### API

| Property             |  Type   | Value            |
|----------------------|---------|------------------|
| open()  | method  | Opens the widget bottom right and starts a session |  
| close()  | method  | Closes the widget window completely |
| showLauncher()  | method  | Show the widget launcher |
| hideLauncher()  | method  | Hide the widget launcher |


## Angular 1 apps
### Show/hide widget depending on certain pages
In order to show or hide the widget on certain page in Angular 1 apps you'll need to listen to route changes.
Using the `$routeChangeSuccess` from the `$route` API (https://docs.angularjs.org/api/ngRoute/service/$route#$routeChangeSuccess) you can change the style to show or hide it.

For example, if you need to hide the widget on a `routeWhereWidgetIsHidden` route your code would look like:
```
$scope.$on('$routeChangeSuccess', function(event, current) {
    if (current.title === 'routeWhereWidgetIsHidden') {
        document.getElementsByClassName('glr-launcher')[0].style.display = 'none';
    } else {
        document.getElementsByClassName('glr-launcher')[0].style.display = 'block';
    }
});
```
