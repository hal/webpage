title: Look & Feel
date: 2013-09-24 08:37:43
layout: page
---

### Widgets

HAL builds on the default GWT widgets [1], with minor modifications to provide a consistent look&feel (css references, image bundles, etc)

For specific interaction types we've implemented dedicated widgets, that go beyond what GWT has to offer. I.e. the form input widget that provides an open-save-close model is good example of this.

If a widget has a general purpose then it belongs into the Ballroom library. If a widget only serves our very specific project needs, we keep it within the core/gui module.

1. http://www.gwtproject.org/doc/latest/RefWidgetGallery.html
2. https://github.com/hal/ballroom

### Accessibility
The web console implements the W3C Aria specification [1] that defines a way to make Web content and Web applications more accessible to people with disabilities.

The overall layout and each specific widget are designed[2] to be composed and provide keyboard access and screen reader support across the board.

> <i class="icon-exclamation-sign"></i> This is an important requirement to pass the 508 compliance [3] tests.
>Please keep this in mind when developing new widgets, layout types or navigation concepts.

1. http://www.w3.org/WAI/intro/aria
2. http://www.w3.org/TR/wai-aria-practices/
3. https://www.section508.gov

