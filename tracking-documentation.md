# Tracking documentation
We use Redux Beacon to handle event tracking (with Google Tag Manager):

[Redux Beacon](https://github.com/rangle/redux-beacon)

This allows to hook our events on the *Redux actions* that we are already using.

The *Beacon* middleware is created inside `src/stores/configureStore.js`, receiving `eventsMap` as the first parameter, which is the events map that we define in `src/redux-modules/tracking.js`.

This `eventsMap` contains an object where the `keys` are the redux `actions` that we want to track and the `value` are functions that return `event` objects.

Each event object is created with the following function definition (provided by Redux Beacon as [EventDefinition](https://rangle.github.io/redux-beacon/docs/api/event-definition.html):
```
export type EventDefinition = (
  action: Action,
  prevState: any,
  nextState: any
) => any | Array<any>;
```

We can also conditionally create events if we need to trigger an event on a certain action depending on the payload, as shown here:
```
function (action, prevState) {
  if (action.payload.location.route === '/404') {
    return null;
  }
  return {
    hitType: action.type.toLowerCase(),
    route: action.payload.location.pathname,
    referrer: prevState.currentRoute,
  }
};
```

Example: if there's no need to track with payload, (we just one to track the event itself)

Tracking the event of clicking the *Contact* button on the widget titlebar:

```
const EVENTS = {
  escalate: () => ({
    event: 'widget_contact_topbar_activated'
  });
}

export const eventsMap = {
  'app/ESCALATE': EVENTS.escalate
}
```
The event `id` used in `EVENTS.escalate` (widget_contact_topbar_activated in this case) has to be provided in the ticket requesting the tracking.
The `eventsMap` key `app/ESCALATE` is the `action` we are already dispatching in the application (can be inspected in Redux devtools easily).

If we want to trigger a custom event that is not tied to any current Redux `action` we can do it as:

Adding: 
`'EVENT_ID': Events.newEvent`
to `eventsMap`,
```
newEvent: () => ({
  event: 'gtm_event_id'
});
```
to `EVENTS`,
and then dispatching the action when we want to track it using `EVENT_ID`:

`store.dispatch({ type: 'EVENT_ID', payload: {}});`

