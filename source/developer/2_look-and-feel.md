title: Look & Feel
date: 2013-09-24 08:37:43
layout: page
---

### Widgets

HAL builds on the default [GWT widgets](http://www.gwtproject.org/doc/latest/RefWidgetGallery.html), with minor modifications to provide a consistent look&feel (css references, image bundles, etc)

For specific interaction types we've implemented dedicated widgets, that go beyond what GWT has to offer. I.e. the form input widget that provides an open-save-close model is good example of this.

If a widget has a general purpose then it belongs into the [Ballroom library](https://github.com/hal/ballroom). If a widget only serves our very specific project needs, we keep it within the core/gui module.


### Look & Feel

HAL provides two different themes: The default one is the community look&feel used with Wildfly. It's enabled and used out of the box.
In order to switch to the product look&feel, you need to rebuild the web console using the `-Peap` maven profile.
This will create  a console build that relies on the [RCUE](http://uxd-rcue.rhcloud.com) styles.

The themes are driven by a [Less CSS](http://lesscss.org) build step that creates two distinct css file for inclusion with the final console.


### Accessibility
The web console implements the [W3C Aria specification](http://www.w3.org/WAI/intro/aria) that defines a way to make Web content and Web applications more accessible to people with disabilities.

The overall layout and each specific [widgets are designed](http://www.w3.org/TR/wai-aria-practices/) to be composed and provide keyboard access and screen reader support across the board.

<div class="alert">
It is an important requirement to pass the [508 compliance](https://www.section508.gov) tests.
Please keep this in mind when developing new widgets, layout types or navigation concepts.
</div>


