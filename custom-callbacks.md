## Custom callbacks

This is a custom callback you can use to trigger your own methods when a user closes the widget. We send the organisation ID to your callback as a single parameter so you can use it if you need it.



`kare.onClose(callback)`

**Example**
             
```
kare.onClose(function(orgId) {
  console.log('my custom close callback');
});

```

## IMPORTANT:

Custom callbacks must be added **after** our widget has loaded.

Notice how we add our callbacks after the event `load` of our script is called.

```
  <script id="kare-widget">
      window.GLR = {
        appId: '190931b6-d139-4c2b-a3ff-144ad89bc38a'
      };
      (function(w, d, s){
        var j = document.createElement(s); j.async = 1; j.type = 'text/javascript'; j.src = 'https://widget.stg.karehq.com/latest.js';
        w.GLR = w.GLR || {};
        d.getElementsByTagName('head')[0].appendChild(j);

        j.addEventListener('load', () => {
          kare.onClose(function(orgId) {
            console.log('my custom close callback');
          });
          kare.onEscalate(function(orgId) {
            console.log('my custom escalate callback');
          });
          kare.onOpen(function(orgId) {
            console.log('my custom open callback');
          });
        });
      })(window, document, 'script');
      
    </script>
```

Keep in mind the browser compatibility you want to provide your users when using this method to trigger your own callback.

We don't manipulate the code inside the callback in any way.