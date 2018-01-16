This documentation covers the different ways you can setup and present Gluru MIND widget on your website.

The widget comes in two variants, the first is `docked` and sits in the bottom right of the page and the other is `dialog`, which sits in the center of the page. It can be triggered with a button launcher (default), binding to a search form, or launching via calling the javascript API.

# Installation

Before you can use the widget you will need to add the following snippet to the website you wish to track. This will enabled the docked widget with button launcher by default. See [Advanced Configuration](#advanced-configuration) to change this behaviour.

```
<script>
window.GLR = {
  appId: 'APPID-FROM-DASHBOARD',
  subDomain: 'subdomain',
};

(function(w, d, s){
  var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://static.glurucdn.com/1.0.0-rc.3/gluru.js';
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
  appId: 'APPID-FROM-DASHBOARD',
  subDomain: 'subdomain',
  showLauncher: false,      // Hide the button launcher 
};

(function(w, d, s){
  var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://static.glurucdn.com/1.0.0-rc.3/gluru.js';
  w.GLR = w.GLR || {};
  d.getElementsByTagName('head')[0].appendChild(j);
})(window, document, 'script');
</script>

```

## Z-Index Issues
If you find that there is some incorrect overlapping of elements on your website and the AskBar then you may need to adjust its Z-Index.

The Z-Index is a CSS property that allows you to adjust the z-order of elements on a page. You set the Z-Index to a number and and element with a higher number will appear on top of an element with a lower number.

For more inofrmations see: [https://developer.mozilla.org/en-US/docs/Web/CSS/z-index](https://developer.mozilla.org/en-US/docs/Web/CSS/z-index).

You can adjust the Z-Index (and other CSS properties) of the AskBar in the following ways:

### Using inline styles
```
<form data-gluru-ask style="z-index: 300;">
    <input type="text" spellcheck>
    <input type="submit" value="Ask a question">
</form>
```

### Using external stylesheets
```
<form class="my-ask-bar" data-gluru-ask style="z-index: 300;">
    <input type="text" spellcheck>
    <input type="submit" value="Ask a question">
</form>
```
Then in your stylesheet:
```
.my-ask-bar {
  z-index: 300;
}
```

# Launching Programmatically 

The widget can be launched in all variants through the Javascript API, the API can be accessed via the `window.gluru`.

## Example

```
window.gluru.openDialog({
   query: 'How can I review my order?',
});

```

## API

| Property             |  Type   | Value            |
|----------------------|---------|------------------|
| showDialog({query})  | method  | Shows the dialog and starts a chat with "query" |  
| onInit(cb)           | method  | NOT IMPLEMENTED  |



# Advanced configuration

Configuration options to pass to `window.GLR` before widget setup.

|  Property    |   Type    |  Description |
|--------------|-----------|--------------|
| key          |   string  | Your client key. Required. |
| showLauncher |   bool    | Show or hide the button launcher | 
| subDomain    |   string  | Your site name, e.g gluru, should match dashboard settings. Not required if using `key`|
| apiBase      |   string  | (Internal) Changes API host
