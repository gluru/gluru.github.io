
# Intercom

Please follow these integration steps:
1. Customize the following script as needed to fit your requirements.
2. Load intercom in your page.
3. Import this script immediately after that.

For more information see [Hubspot developer portal](https://developers.hubspot.com/docs/api/conversation/chat-widget-sdk)


```javascript
// Hide Intercom when needed. 
function hideIntercom() {
    Intercom('hide');
    Intercom('update', {"hide_default_launcher": true});
}

// Show Intercom when needed. 
function showIntercom() {
    Intercom('update', {"hide_default_launcher": false});
}

window.GLR = {
    appId: '<APP-ID>',
    host_locale: 'en-SG'
};
(function (w, d, s) {
    var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
    w.GLR = w.GLR || {};
    d.getElementsByTagName('head')[0].appendChild(j);

    j.addEventListener('load', () => {
        kare.onClose(function (callbackEvent) {
            showIntercom();
        });
        kare.onEscalate(function (callbackEvent, messageBody) {
            kare.close();
            kare.hideLauncher();
            showIntercom();
        });
        kare.onOpen(function (callbackEvent) {
            // If Intercom coexist with Kare we can close it when Kare
            // is opened. 
            // hideIntercom();
        });
        kare.onBeforeSocketOpen(function (callbackEvent) {
            console.log('my custom onBeforeSocketOpen callback');
        });

        // Hide Intercom by default.
        // Alternatively it can be hidden when Kare opens.
        hideIntercom();
    });
})(window, document, 'script');
```

