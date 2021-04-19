# Gorgias

 1. Deploy Gorgias in a page.
 2. In the Kare console enable the `custom` escalation point.
 3. Customize the Kare script as follows.


Fore more information about how to customize your Gorgias integration please check the [Gorgias' advanced settings page](https://docs.gorgias.com/gorgias-chat/advanced-customization-new-chat).


```
func hideGorgias() {
  var gor = document.getElementById("chat-button");
  gor.style.display = "none"
}

window.GLR = {
    appId: '<YOUR-APP-ID>'
    };
    (function(w, d, s){
    var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
    w.GLR = w.GLR || {};
    d.getElementsByTagName('head')[0].appendChild(j);

    j.addEventListener('load', () => {
        kare.onClose(function(callbackEvent) {
        console.log('my custom onClose callback');
        });
        kare.onEscalate(function(callbackEvent, messageBody) {
          console.log('my custom onEscalate callback');
          gor.style.display = "block";
          window.GorgiasChat.open();
          kare.close();
          kare.hideLauncher();
        });
        kare.onOpen(function(callbackEvent) {
          console.log('my custom onOpen callback');
        });          
        kare.onBeforeSocketOpen(function(callbackEvent) {
          console.log('my custom onBeforeSocketOpen callback');
        });

        hideGorgias();
        window.GorgiasChat.close();
        window.GorgiasChat.on('widget:closed', function(data) {
            console.log('windows.GorgiasChat.off');
            hideGorgias();
            kare.showLauncher();
        })
    });
    })(window, document, 'script');
```
