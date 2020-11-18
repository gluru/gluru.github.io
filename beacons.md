## Beacons

Beacons is a new feature available to increase engagment of your users. Allows you to add another entry point to the conversation with a specific response attached to it. A beacon can be useful to start a conversation with a specific response related with what the user is concerned about.

For instance, it would be possible to add a little _tooltip_ in every FAQ entry to expand information for that FAQ. 

# Syntax

### window.kare.addBeacon(_beaconConfiguration_)


## BeaconConfiguration object fields
 > beaconName
    
Arbitrary name to identify the beacon in your webpage. This has to be unique. Required
  
 > beaconType

Type of the beacon. One of the following ['tooltip', 'search', 'other']. Required

 > welcomeId

Response id used to link this beacon and the conversation. Every click to this beacon will trigger to render that response in the widget. Required one between `welcomeId` and `previewId`
  
 > previewId

Response id used to link this beacon as a popup. A click to this beacon will open a popoup rendering the response. Required one between `welcomeId` and `previewId`

 > htmlClickableElementId

Id of the html element that is going to beacome the visible part of the beacon. Required

 > onLoad

Callback to be call when the beacon is correctly parsed and can be rendered to the user. Optional

 > onOpen

Callback to be call when the beacon is clicked. Optional

 > onFail

Callback to be call when the beacon could not be parsed correctly. Useful to track not working beacons. Optional


# Example

A beacon is composed of two parts:

Any html clickable element in your website:

```
<! demo.html>
<head>
...
</head>
<body>
...
    <div id="billing-beacon" class="karehq-tooltip"/>
</body>
```

An a piece of javascript code you need to call in order to register that html beacon to the widget.

```
<script>
window.onload = () => {

  // Hide the html element that will represent the beacon until the SDK calls the onLoad callback to confirm the beacon is working.
  var beacon = document.getElementById('billing-beacon');
  beacon.style.visibility = 'hidden';

  // Register the beacon to the SDK
  window.kare.addBeacon({
    beaconName: 'test-beacon', // Beacon unique name across the page
    beaconType: 'tooltip', // Beacon type. One between ['tooltip', 'search', 'other']
    welcomeId: 'node_c3cac5a08df51e825af0d86ebf6a52eb', // Response id this beacon will use
    htmlClickableElementId: 'billing-beacon', // HTML Element id that will represent this beacon
    onLoad: () => { // Callback called when the beacon is correct
      element.style.visibility = 'visible';
    },
    onOpen: () => { // Callback called when the beacon is clicked
      element.style.backgroundColor = '#ccccff';
    }
  });
}
</script>
```
