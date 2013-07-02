WAAClock
==========

Web Audio API doesn't provide a comprehensive API for scheduling things in the time : 

  - it is hard or impossible to cancel events once they have been scheduled
  - there is no way to schedule a custom event
  - there is no way to schedule repeating events
  - ...

`WAAClock` adds a very thin layer allowing you play with the time in a more easy way.

First download the latest stable release of `WAAClock.js` from [dist/](https://github.com/sebpiq/WAAClock/tree/master/dist), then create an `AudioContext` and a `WAAClock` :

```javascript
var context = window.AudioContext ? new AudioContext() : new webkitAudioContext()
  , clock = new WAAClock(context)
```

(Prepare a function for the demos :)

```javascript
var printer = function(msg) { return function() { console.log(msg) } }
```

And now you can schedule (approximatively) custom events :

```
var event = clock.setTimeout(printer('wow!'), 13)
```

Schedule (exactly), and cancel built-in Web Audio API events :

```
var osc = context.createOscillator()
  , startEvent = osc.start2(5)
  , freqChangeEvent = osc.frequency.setValueAtTime2(220, 5)

clock.setTimeout(function() {
  startEvent.clear()
  freqChangeEvent
}, 4)
```

You can set events to repeat periodically :

```javascript
var event = clock.setTimeout(printer('wow!'), 3).repeat(2)
```

Change the tempo of a group of events :

```javascript
var event1 = clock.setTimeout(printer('wow!'), 1).repeat(2)
  , event2 = clock.setTimeout(printer('what?'), 2).repeat(2)
  , tempoChange = clock.setTimeout(function() {
    clock.timeStretch([event1, event2], 0.5)
  }, 10)
```

Examples
---------

Check-out this [simple repetitive pattern](http://sebpiq.github.io/WAAClock/tempoChange.html) or a [basic sequencer](http://sebpiq.github.io/WAAClock/beatSequence.html).

API
----

TBD

License
--------

Released under MIT license

Change log
-----------
