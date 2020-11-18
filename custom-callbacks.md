## List of current available callbacks
### kare.onClose
Executed when the widget is minimized
### kare.onEscalate
Executed when the user escalates
### kare.onOpen
Executed when the widget opens (normally when the user clicks on the FAB)
### kare.onBeforeSocketOpen
Executed just before we try to connect to the websocket. This connection is not established if the user does not open the widget.

## IMPORTANT:
Custom callbacks must be added **after** our widget has loaded.

Notice how we add our callbacks after the event `load` of our script is called.

```
  <script id="kare-widget">
      window.GLR = {
        appId: '190931b6-d139-4c2b-a3ff-144ad89bc38a'
      };
      (function(w, d, s){
        var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.eu.karehq.com/latest.js';
        w.GLR = w.GLR || {};
        d.getElementsByTagName('head')[0].appendChild(j);

        j.addEventListener('load', () => {
          kare.onClose(function(orgId) {
            console.log('my custom onClose callback');
          });
          kare.onEscalate(function(orgId) {
            console.log('my custom onEscalate callback');
          });
          kare.onOpen(function(orgId) {
            console.log('my custom onOpen callback');
          });          
          kare.onBeforeSocketOpen(function(orgId) {
            console.log('my custom onBeforeSocketOpen callback');
          });
        });
      })(window, document, 'script');
      
    </script>
```

Keep in mind the browser compatibility you want to provide your users when using this method to trigger your own callback.

We don't manipulate the code inside the callback in any way.