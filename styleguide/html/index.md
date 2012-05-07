---
subtitle: HTML
layout: styleguide
---

HTML Style Guide
================

Formatting
----------

*TODO*

Doctype and layout
------------------

All documents should be using the HTML5 doctype and the `<html>` element should
have a `"lang"` attribute. The `<head>` should also at a minimum include `"viewport"`
and `"charset"` meta tags.

    <!doctype html>
    <html lang="en">
      <head>
        <meta charset="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <title>Example Site</title>
      </head>
      <body></body>
    </html>
{:html}

Including meta data
------------------

Classes should ideally only be used as styling hooks. If you need to include
additional data in the html document, for example to pass data to JavaScript,
then the HTML5 `data-` attributes should be used.

{:html}
    <a class="btn" data-format="csv">Download CSV</a>

These can then be accessed easily via jQuery using the `.data()` method.

    jQuery('.btn').data('format'); //=> "csv"

    // Get the contents of all data attributes.
    jQuery('.btn').data(); => {format: "csv"}
{:js}

One thing to note is that the JavaScript API for datasets will convert all
attribute names into camelCase. So `"data-file-format"` will become `fileFormat`.

For example:

    <a class="btn" data-file-format="csv">Download CSV</a>
{:html}

Will become:

    jQuery('.btn').data('fileFormat'); //=> "csv"
    jQuery('.btn').data(); => {fileFormat: "csv"}
{:js}

Targeting Internet Explorer
---------------------------

Targeting lower versions of Internet Explorer (IE), those below version 9,
should be handled by the stylesheets. Small fixes should be provided inline
using the `.ie` specific class names. Larger fixes may require a separate
stylesheet but try to avoid this if at all possible.

Adding IE specific classes:

    <!doctype html>
    <!--[if lt IE 7]> <html lang="en" class="ie ie6"> <![endif]-->
    <!--[if IE 7]>    <html lang="en" class="ie ie7"> <![endif]-->
    <!--[if IE 8]>    <html lang="en" class="ie ie8"> <![endif]-->
    <!--[if gt IE 8]><!--> <html lang="en"> <!--<![endif]-->
{:html}

_NOTE: Only add lines for classes that are actually being used._

These can then be used within the CSS:

    .clear:before,
    .clear:after {
        content: "";
        display: table;
    }

    .clear:after {
        clear: both;
    }

    .ie7 .clear {
        zoom: 1; /* For IE 6/7 (trigger hasLayout) */
    }
{:css}

{:html: data-code="html"}
{:css: data-code="css"}
{:js: data-code="js"}
