# rb_smil_emulator.js 

## Abstract

* File name: `rb_smil_emulator.js`
* Version: 1.8
* Date: 2013-09-12
* Author: Alberto Pettarin ([website](http://www.albertopettarin.it/), [contact information](http://www.albertopettarin.it/contact.html))
* License: The MIT License (MIT), see LICENSE.md

This JavaScript enables the tap-to-play function on those platforms
that do not properly support the EPUB 3 Media Overlay specification
(most notably, Apple iBooks).

Please note that this JS script is independent from the integrated audio controls
and the regular Media Overlay/SMIL controls that your reading system might provide,
and it might conflict with them.
Use it at your own risk!
If you think that tap-to-play is a useful feature,
I request you to urge the developers of your favourite reading system to implement
the official EPUB 3 Media Overlay specification.

Although primarily developed for iBooks, it works in Readium
(to which it adds the still not implemented tap-to-play function)
and in modern browsers (e.g., Chrome) as well.


## Usage

In your XHTML page (say, `page.xhtml`) you must add the following code blocks.

In the `<head>`:
```HTML
<script type="text/javascript" src="path/to/rb_smil_emulator.js"></script>
<script type="text/javascript" src="path/to/page.smil.js"></script>
```

As the last element of `<body>`
(see below for optional parameters in the form `{k1: p1, k2: p2, k3: p3}`):
```HTML
<script type="text/javascript">
//<![CDATA[
  window.addEventListener('DOMContentLoaded', 
    function() {
      window.rb_smil_emulator.init('path/to/page.mp3', {k1: p1, k2: p2, k3: p3});
    }
  );
//]]>
</script>
```

Remember to:
* define two CSS classes to style the active or paused SMIL fragment
(the default names are `rbActiveFragment` and `rbPausedFragment`, respectively), and
* generate `page.smil.js`, containing `smil_data` and `smil_ids`.

The SMIL fragments, defined in `page.smil.js`,
must be contiguous
(the end of the i-th fragment coincides with
the begin of the (i+1)-th fragment),
and span the entire audio track
(the first fragment should start at time zero,
while the last fragment should end
at the end of the audio track).
Fragments might have zero duration
(i.e., their begin and end time coincide).

Please observe that `smil_data.length`
should be equal to `smil_ids.length`,
and it should be equal to the number of SMIL fragments;
the fragments must appear in these two arrays
sorted according to their begin time.
Fragment IDs might be arbitrary (but unique) strings.

Optional parameter keys include:
* `active_fragment_class_name`
* `paused_fragment_class_name`
* `autostart_audio`
* `autostart_wait_event`
* `autoturn_page`
* `single_fragment`
* `outside_taps_clear`
* `outside_taps_can_resume`
* `outside_taps_threshold`
* `associated_events`
* `ignore_taps_on_a_elements`
* `allowed_reading_systems`

The meaning of the above options, their type and default value
are described in the comment right above `init()` in the source code.


## Source code

Please see the `src/` directory.


## Examples

A minimal (but working) example can be found in the `minimal/` directory.

Two full Audio-eBook examples can be found online at [Smuuks web site](http://www.smuuks.it/index.php/en/):
* _The Curious Case of Benjamin Button_, by F. Scott Fitzgerald, in English, 1h audio/645 SMIL fragments: [here](http://www.smuuks.it/index.php/en/projects/the-curious-case-of-benjamin-button)
* _Cuore_, by E. De Amicis, in Italian, 9h audio/6,762 SMIL fragments: [here](http://www.smuuks.it/index.php/it/progetti/cuore).


## TODO List

* Support generic SMIL fragments (not necessarily contiguous nor spanning the entire audio track)
* Support multiple highlighted elements at the same time
* Integration with existing `<audio>` (or `<video>`) element (unfortunately at the moment seeking seems broken in iBooks)
* More flexible, user-customizable interaction between user click/touch and audio rendition behaviour
* Avoid collision of two audio sources in iBooks (if possible at all --- right now it does not seem possible)
* Use/integrate existing SMIL libraries (e.g., `timesheet.js`)
* Load next chapter upon the end of the current


## Acknowledgments

* I was inspired by the source code of [Readium](http://readium.org/) and [Timesheet.js](http://wam.inrialpes.fr/timesheets/)
* Thanks to _Giulio Cesare Solaroli_ for a useful discussion about JS
* Thanks to _torazaburo_ for answering my question about [how to make iBook turn page with JS](http://stackoverflow.com/questions/16922631/make-ibooks-turn-page-in-reflow-mode-with-javascript-embedded-in-the-displayed)
* Thanks to _Frank Chen_ for providing a sample JS code incorporated into the current code for the autoturnpage option

Please feel free to send me your feedback about this project
(my email address can be found in the source code header),
and/or fork and improve it!

