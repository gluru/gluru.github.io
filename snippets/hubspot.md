
# Hubspot

Please follow these integration steps:
1. make sure that Hubspot is correctly integrated
2. Load the following script. Feel fre to customize it as needed.

For more information see [Hubspot developer portal](https://developers.hubspot.com/docs/api/conversation/chat-widget-sdk)


```javascript
function hideHubspot() {
    window.HubSpotConversations.widget.remove();
}

function loadHubspot(open) {
    window.HubSpotConversations.widget.load({
        widgetOpen: open
    });
    window.HubSpotConversations.on('conversationClosed', payload => {
        console.log('windows.Hubspot.off');
        hideHubspot();
        kare.showLauncher();
    });
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
            console.log('my custom onClose callback');
        });
        kare.onEscalate(function (callbackEvent, messageBody) {
            console.log('my custom onEscalate callback');
            kare.close();
            kare.hideLauncher();
            loadHubspot(true);
        });
        kare.onOpen(function (callbackEvent) {
            console.log('my custom onOpen callback');
        });
        kare.onBeforeSocketOpen(function (callbackEvent) {
            console.log('my custom onBeforeSocketOpen callback');
        });

        hideHubspot();
    });
})(window, document, 'script');
```
