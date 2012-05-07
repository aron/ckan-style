---
subtitle: CSS
layout: styleguide
---

CSS Style Guide
===============

Formatting
----------

All CSS documents must use **two spaces** for indentation and files should have
no trailing whitespace.

Naming
------

All ids, classes and attributes must be lowercase with hyphens used for
separation.

    /* GOOD */
    .dataset-list {}

    /* BAD */
    .datasetlist {}
    .datasetList {}
    .dataset_list {}

Modularity & Specificity
------------------------

Try keep all selectors loosely grouped into modules where possible and avoid
having too many selectors in one declaration to make them easy to override.

    /* Avoid */
    ul#dataset-list {}
    ul#dataset-list li {}
    ul#dataset-list li p a.download {}

Instead here we would create a dataset "module" and styling the item outside of
the container allows you to use it on it's own e.g. on a dataset page:

    .dataset-list {}
    .dataset-list-item {}
    .dataset-list-item .download {}

In the same vein use classes make the styles more robust, especially where the
HTML may change. For example when styling social links:

    <ul class="social">
      <li><a href="">Twitter</a></li>
      <li><a href="">Facebook</a></li>
      <li><a href="">LinkedIn</a></li>
    </ul>

You may use pseudo selectors to keep the HTML clean:

    .social li:nth-child(1) a {
      background-image: url(twitter.png);
    }

    .social li:nth-child(2) a {
      background-image: url(facebook.png);
    }

    .social li:nth-child(3) a {
      background-image: url(linked-in.png);
    }

However this will break any time the HTML changes for example if an item is
added or removed. Instead we can use class names to ensure the icons always
match the elements (Also you'd probably sprite the image :).

    .social .twitter {
      background-image: url(twitter.png);
    }

    .social .facebook {
      background-image: url(facebook.png);
    }

    .social .linked-in {
      background-image: url(linked-in.png);
    }

Avoid using tag names in selectors as this prevents re-use in other contexts.

    /* Cannot use this class on an <ol> or <div> element */
    ul.dataset-item {}

Also ids should not be used in selectors as it makes it far too difficult to
override later in the cascade.

    /* Cannot override this button style without including an id */
    .btn#download {}

Resources
---------

- [OOCSS][#oo]
- [An Introduction to Object Orientated CSS][#smashing-oo]
- [SMACSS][#smacss]
- [CSS for Grown Ups][#grownups] ([slides][#grownups-slides])

_NOTE: These resources are more related to structuring CSS for large projects rather
than actual how-to style guides._

[#oo]: http://www.stubbornella.org/content/2011/04/28/our-best-practices-are-killing-us/
[#smashing-oo]: http://coding.smashingmagazine.com/2011/12/12/an-introduction-to-object-oriented-css-oocss/
[#smacss]: http://smacss.com
[#grownups]: http://schedule.sxsw.com/2012/events/event_IAP9410
[#grownups-slides]: http://speakerdeck.com/u/andyhume/p/css-for-grown-ups-maturing-best-practises
