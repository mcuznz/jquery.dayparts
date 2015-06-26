jquery.dayparts
===============

A jQuery Plugin for selecting Hours-of-Day.  Demo at http://mcuznz.ca/2014/05/jquery-dayparts-plugin-for-selecting-hours-of-the-day/
I'll improve on the Demo later to display extra options and catching the update event.

Consider this a Beta at best; it works for the single use case that I needed it for, but might not work in all cases.  It also needs some prettying up.

Call on an empty element such as a `<div>`, and it'll be populated with a Daypart selection table.

Note that this Plugin doesn't directly modify the content of a provided `<input>`, as it returns an array of Objects.  It does, however, trigger a catchable 'daypartsUpdate' event when changes are made, which you cna listen to, fetch the resulting data, and morph accordingly.

I'll provide better documentation soon, I swear.


Configuration
-------------

All options can have their defaults overridden (overrode?) and also can be specified per-instance.

```javascript
$.fn.dayparts.defaults = {
    disabled: false, 
    i18nfunc: function(input){ return input; },
    days: {0: 'Sunday', 1: 'Monday', 2: 'Tuesday', 3: 'Wednesday', 4: 'Thursday', 5: 'Friday', 6: 'Saturday'},
    weekStartsOn: 0,
    use24HFormat: true,
    change0Hour: true,
    show2DigitsHour: false,
    showPresets: true,
    presets: [
        {label:"Full Coverage", days:[0,1,2,3,4,5,6], hours:[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23]},
        {label:"Afternoons", days:[0,1,2,3,4,5,6], hours:[12,13,14,15,16,17]},
        {label:"Evenings", days:[0,1,2,3,4,5,6], hours:[18,19,20,21,22,23]},
        {label:"Mornings", days:[0,1,2,3,4,5,6], hours:[6,7,8,9,10,11]},
        {label:"Weekdays", days:[1,2,3,4,5], hours:[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23]},
        {label:"Weekends", days:[0,6], hours:[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23]},
        {label:"Weekends including Friday", days:[0,5,6], hours:[0,1,2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20,21,22,23]}
    ],
    labels: {
        am: 'AM',
        pm: 'PM',
        presets: 'Presets',
        presetsSubtitle: '',
        choosePreset: 'Select a Preset'
    },
    data: []
};
```

__Worth noting:__ all text - days of the week, preset names, and labels - is passed through the provided `i18nfunc` before being displayed.  If this isn't behaviour you need, leave the default `i18nfunc` alone and just provide the values in the language of your choice.  If you're creating a multilingual site, however, simply use Translation Keys in place of the Label/Day/Preset text, and pass your i18n Translator function as the `i18nfunc` option.

`data` is whatever selected dayparts you want to have initially selected.  It takes the form of an array of objects as follows:
```javascript
[{day: 0, hour: 23}, {day: 1, hour: 0}]
```
Values for `day` are 0-6 (Sunday to Saturday) and values for `hour` are 0-23.

This is an example to generate a `Full Coverage` `data`:
```javascript
var data = [];
for (var day=0;day<7;day++){
    for (var hour=0;hour<24;hour++){
        data[data.length]={day: day, hour: hour};
    }
}
```

Most other options are self-explanitory.

__A note about Timezones:__ You're on your own here!


Listening for Changes:
----------------------

You'll want to set this up something like the following:
```javascript
$(selector).dayparts({data: [...]});
$(selector).on("daypartsUpdate", function() {

  var parts = $(selector).dayparts.getValue();
  // Your Handler here - recurse over parts and morph as necessary.

});
```

Known Bugs
----------

You can drag from one block to another to "paint" a section on or off, depending on the current state of the initial block. If you drag outside of the table and release the mouse, the Dayparting widget doesn't catch the resulting `mouseup` event, and sticks in paint mode. Clicking once will escape it, but you won't get the paint results you expected.


That's all she wrote!
