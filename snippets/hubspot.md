
# Hubspot

Please follow these integration steps:
1. Customize the following script as needed to fit your requirements.
2. Load this script before Hubspot in order to properly set the initialization parameters. See [Preventing Hubspot from loading immediately](https://community.hubspot.com/t5/APIs-Integrations/Prevent-bot-from-loading-immediately/m-p/297539).
3. Load Hubspot chat.

For more information see [Hubspot developer portal](https://developers.hubspot.com/docs/api/conversation/chat-widget-sdk)


```javascript
// Configure hubspot to load without displaying the FAB.
// This should ideally be executed before Hubspot.
if (window.HubSpotConversations) {
    indow.hsConversationsSettings = {
        loadImmediately: false,
    };
 }else {
    window.hsConversationsOnReady = [
        () => {
            window.hsConversationsSettings = {
                loadImmediately: false,
            };
        },
    ];
}


// Hide Hubspot when needed. 
function hideHubspot() {
    window.HubSpotConversations.widget.remove();
}

// Show Hubspot when needed. 
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

