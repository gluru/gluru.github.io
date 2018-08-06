This documentation covers the different ways you can setup and present Gluru MIND widget on your website.

The widget comes in two variants, the first is `docked` and sits in the bottom right of the page and the other is `dialog`, which sits in the center of the page. It can be triggered with a button launcher (default), binding to a search form, or launching via calling the javascript API.

# Installation

Before you can use the widget you will need to add the following snippet to the website you wish to track. This will enabled the docked widget with button launcher by default. See [Advanced Configuration](#advanced-configuration) to change this behaviour.

```
<script>
window.GLR = {
  appId: 'APPID-FROM-CONSOLE',
};

(function(w, d, s){
  var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://s3.eu-west-1.amazonaws.com/static.glurucdn.com/head/gluru.js';
  w.GLR = w.GLR || {};
  d.getElementsByTagName('head')[0].appendChild(j);
})(window, document, 'script');
</script>
```

# AskBar 

__Note, the search bar is still in early development and the API around it is likely to change.__

Add the `data-gluru-ask` attribute to a form to get Gluru to bind to it. Once an `onsubmit` event is fired, gluru will open the widget as a dialog variant pre-filled with a question. The question will be taken from the first text input inside the form. Below is an example of using the form handler.

```

<script>

<form data-gluru-ask>
    <input type="text" spellcheck>
    <input type="submit" value="Ask a question">
</form>

window.GLR = {
  appId: 'KEY-FROM-CONSOLE',
  showLauncher: false,      // Hide the button launcher 
};

(function(w, d, s){
  var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://s3-eu-west-1.amazonaws.com/gluru-widget/head/gluru.js';
  w.GLR = w.GLR || {};
  d.getElementsByTagName('head')[0].appendChild(j);
})(window, document, 'script');
</script>

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

## Inside a web page

```
<button onclick="window.gluru.openDialog();">Open help window as dialog</button>
<button onclick="window.gluru.open();">Open at side of page</button>
<button onclick="window.gluru.open({ query: 'How can I track my order?' });">Open with query</button>
<button onclick="window.gluru.close();">Close the widget</button>
```

## API

| Property             |  Type   | Value            |
|----------------------|---------|------------------|
| open()  | method  | opens the widget bottom right and starts a session |  
| close()  | method  | closes the widget window completely 


# Advanced configuration

Configuration options to pass to `window.GLR` before widget setup.

|  Property    |   Type    |  Description |
|--------------|-----------|--------------|
| key          |   string  | Your client key. Required. |
| showLauncher |   bool    | Show or hide the button launcher | 
| siteUrl      |   string  | URL to your main website. Not required if using `key` |
| escalateUrl  |   string  | URL to send customers if they escalate | 
| apiBase      |   string  | (Internal) Changes API host
| highlightArticleHits | bool | **experimental** Default: false. If enabled, highlights hit sentences when on the resolution link page |

# Angular 1 apps
## Show/hide widget depending on certain pages
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
