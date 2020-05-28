## Widget launcher time schedule code snippet

This snippet will change the behaviour of the widget launcher button. Given a list of open hours, the widget launcher button will appear only in those specified hours.

The script is pretty simple. We create a dictionary `openHours` where the key is the name of the day of the week, and the value is a list of hour ranges. When the current time is inside one of those ranges, the launcher will be shown, otherwise hid.

For instance, if we want to show the launcher button only during the morning, (i.e from 9 to 13) from Monday to Friday we should specify a `openHours` dictionary with the following values:

```javascript
var openHours = {
      sunday: [],
      monday: [[9, 13],
      tuesday: [[9, 13]],
      wednesday: [[9, 13]],
      thursday: [[9, 13]],
      friday: [[9, 13]],
      saturday: [[0, 23], []]
    };
```

```javascript class:"lineNo"
window.GLR = {
  appId: 'app-id-of-the-organisation'
};
(function(w, d, s) {
  var j = document.createElement(s);
  j.async = 1;
  j.type = 'text/javascript';
  j.src = 'https://widget.stg.karehq.com/latest.js';
  w.GLR = w.GLR || {};
  d.getElementsByTagName('head')[0].appendChild(j);
  j.addEventListener('load', () => {
    var days = {
      0: 'sunday',
      1: 'monday',
      2: 'tuesday',
      3: 'wednesday',
      4: 'thursday',
      5: 'friday',
      6: 'saturday'
    };
    //Add any number of hour ranges.
    var openHours = {
      sunday: [[0, 9], [22, 23]],
      monday: [[0, 9], [22, 23]],
      tuesday: [[0, 9], [18, 23]],
      wednesday: [[0, 9], [18, 23]],
      thursday: [[0, 9], [18, 23]],
      friday: [[0, 9], [18, 13]],
      saturday: [[0, 9], [18, 23]]
    };
    var d = new Date();
    var day = days[d.getDay()];
    var currentHour = d.getHours();
    var hourRanges = openHours[day];
    window.kare.hideLauncher();
    for (let i = 0; i < hourRanges.length; i++) {
      var range = hourRanges[i];
      if (range[0] <= currentHour && currentHour <= range[1]) {
        window.kare.showLauncher();
        break;
      }
    }
  });
})(window, document, 'script');
```
