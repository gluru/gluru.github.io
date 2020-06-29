# Escalate to Intercom
Custom pre populated message
```
 kare.onEscalate(e => {
  const Intercom = window.Intercom;
  if (Intercom) {
    /**
     * Open Intercom with a pre populated message
     **/
    Intercom('showNewMessage', 'pre poulated message');
  } else {
    console.log('Intercom has not loaded');
  }
});
```

Using last Kare Widget message to prepopulate Intercom
```
 kare.onEscalate(e => {
    const Intercom = window.Intercom;
    if (Intercom) {
      /**
       * Open Intercom with a pre populated message
       **/
      if (lastMessages && lastMessages.length > 0) {
        let lastMessage = null;
        if (lastMessages.length === 1) {
          lastMessage = lastMessages[0];
        }
        if (lastMessages.length > 1) {
          lastMessage = lastMessages.slice(-1)[0];
        }
        Intercom('showNewMessage', lastMessage);
      } else {
        Intercom('show');
      }
    } else {
      console.log('Intercom has not loaded');
    }
  });
  
```
