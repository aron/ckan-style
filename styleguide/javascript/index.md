---
layout: styleguide
subtitle: JavaScript
---

JavaScript Style Guide
======================

- Table of Contents
{:toc}

Formatting
----------

All JavaScript documents must use **two spaces** for indentation and files
should have no trailing whitespace. This is contrary to the [OKFN Coding
Standards][#okf] but matches what's in use in the current code base.

Coding style must follow the [idiomatic.js][#ideo] style but with the following
exceptions.

_NOTE: Idiomatic is heavily based upon [Douglas Crockford's][#crock] style
guide which is recommended by the OKFN Coding Standards._

### White Space

Two spaces must be used for indentation at all times. Unlike in idiomatic
whitespace must not be used _inside_ parentheses between the parentheses
and their contents.

    // BAD: Too much whitespace.
    function getUrl( full ) {
      var url = '/styleguide/javascript/';
      if ( full ) {
        url = 'http://okfn.github.com/ckan' + url;
      }
      return url;
    }

    // GOOD:
    function getUrl(full) {
      var url = '/styleguide/javascript/';
      if (full) {
        url = 'http://okfn.github.com/ckan' + url;
      }
      return url;
    }
{:js}

_NOTE: See section 2.D.1.1 of idiomatic for more examples of this syntax._

### Quotes

Single quotes should be used everywhere unless writing JSON or the string
contains them. This makes it easier to create strings containing HTML.

    jQuery('<div id="my-div" />').appendTo('body');
{:js}

Object properties need not be quoted unless required by the interpreter.

    var object = {
      name: 'bill',
      'class': 'user-name'
    };
{:js}

### Variable declarations

One `var` statement must be used per variable assignment. These must be
declared at the top of the function in which they are being used.

    // GOOD:
    var good = "string";
    var alsoGood = "another;

    // GOOD:
    var good = "string";
    var okay = [
      "hmm", "a bit", "better"
    ];

    // BAD:
    var good = "string",
        iffy = [
      "hmm", "not", "great"
    ];
{:js}

Declare variables at the top of the function in which they are first used. This
avoids issues with variable hoisting. If a variable is not assigned a value
until later in the function then it it okay to define more than one per statement.

    // BAD: contrived example.
    function lowercaseNames(names) {
      var names = [];

      for (var index = 0, length = names.length; index < length; index += 1) {
        var name = names[index];
        names.push(name.toLowerCase());
      }

      var sorted = names.sort();
      return sorted;
    }

    // GOOD:
    function lowercaseNames(names) {
      var names = [];
      var index, sorted, name;

      for (index = 0, length = names.length; index < length; index += 1) {
        name = names[index];
        names.push(names[index].toLowerCase());
      }

      sorted = names.sort();
      return sorted;
    }
{:js}

[#okf]: http://wiki.okfn.org/Coding_Standards#Javascript
[#crock]: http://javascript.crockford.com/code.html
[#ideo]: https://github.com/rwldrn/idiomatic.js/
[#var]: http://benalman.com/news/2012/05/multiple-var-statements-javascript/

Naming
------

All properties, functions and methods must use lowercase camelCase:

    var myUsername = 'bill';
    var methods = {
      getSomething: function () {}
    };
{:js}

Constructor functions must use uppercase CamelCase:

    function DatasetSearchView() {
    }
{:js}

Constants must be uppercase with spaces delimited by underscores:

    var env = {
      PRODUCTION:  'production',
      DEVELOPMENT: 'development',
      TESTING:     'testing'
    };
{:js}

Event handlers and callback functions should be prefixed with "on":

    function onDownloadClick(event) {}

    jQuery('.download').click(onDownloadClick);
{:js}

Boolean variables or methods returning boolean functions should prefix
the variable name with "is":

    function isAdmin() {}

    var canEdit = isUser() && isAdmin();
{:js}

_Alternatives are "has", "can" and "should" if they make more sense_

Private methods should be prefixed with an underscore:

    View.extend({
      "click": "_onClick",
      _onClick: function (event) {
      }
    });
{:js}

Comments
--------

Comments should be used to explain anything that may be unclear when you return
to it in six months time. Single line comments should be used for all inline
comments that do not form part of the documentation.

    // Export the function to either the exports or global object depending
    // on the current environment. This can be either an AMD module, CommonJS
    // module or a browser.
    if (typeof module.define === 'function' && module.define.amd) {
      module.define('broadcast', function () {
        return Broadcast;
      });
    } else if (module.exports) {
      module.exports = Broadcast;
    } else {
      module.Broadcast = Broadcast;
    }
{:js}

File Structure
--------------

All public JavaScript files should be contained within a _javascript_ directory
within the _public_ directory and files should be structured accordingly.

    lib/
      main.js
      utils.js
      components/
    vendor/
      jquery.js
      jquery.plugin.js
      underscore.js
    templates/
    test/
      index.html
      spec/
        main-spec.js
        utils-spec.js
      vendor/
        mocha.js
        mocha.css
        chai.js

All files and directories should be lowercase with hyphens used to separate words.

 - _lib_: Should contain all application files. These can be structured
   appropriately. It is recommended that _main.js_ be used as the bootstrap
   filename that sets up the page.
 - _vendor_: Should contain all external dependencies. These should not contain
   version numbers in the filename. This information should be available in
   the header comment of the file. Library plugins should be prefixed with the
   library name. eg the hover intent jQuery plugin would have the filename
   _jquery.hover-intent.js_.
 - _templates_: Should be stored in a seperate directory and have the .html
   extension.
 - _test_: Contains the test runner _index.html_. _vendor_ contains all test
   dependencies and libraries. _spec_ contains the actual test files. Each
   test file should be the filename with _-spec_ appended.

JSHint
------

All JavaScript should pass JSHint before being committed. This can be installed
using `node` and `npm` by running:

    $ npm -g install jshint

Each project should include a jshint.json file with appropriate configuration
options for the tool. Most text editors can also be configured to read from
this file.

Documentation
-------------

_TODO_

Testing
-------

_TODO_

Best Practices
--------------

### Forms

All forms should work without JavaScript enabled. This means that they must
submit `x-www-urlencoded` data to the server and receive an appropriate
response. The server should check for the `X-Requested-With: XMLHTTPRequest`
header to determine if the request is an ajax one. If so it can return an
appropriate format, otherwise it should issue a 303 redirect.

The one exception to this rule is if a form or button is injected with
JavaScript after the page has loaded. It's then not part of the HTML document
and can submit any data format it pleases.

### Ajax

Ajax requests can be used to improve the experience of submitting forms and
other actions that require server interactions. Nearly all requests will
go through the following states.

 1.  User clicks button.
 2.  JavaScript intercepts the click and disables the button (add `disabled`
     attr).
 3.  A loading indicator is displayed (add class `.loading` to button).
 4.  The request is made to the server.
 5a. On success the interface is updated.
 5b. On error a message is displayed to the user if there is no other way to
     resolve the issue.
 6.  The loading indicator is removed.
 7.  The button is re-enabled.

Here's a possible example for submitting a search form using jQuery.

    jQuery('#search-form').submit(function (event) {
      var form = $(this);
      var button = form.find('[type=submit]');

      // Prevent the browser submitting the form.
      event.preventDefault();

      button.prop('disabled', true).addClass('loading');

      jQuery.ajax({
        type: this.method,
        data: form.serialize(),
        success: function (results) {
          updatePageWithResults(results);
        },
        error: function () {
          showSearchError('Sorry we were unable to complete this search');
        },
        complete: function () {
          button.prop('disabled', false).removeClass('loading');
        }
      });
    });
{:js}

This covers possible issues that might arise from submitting the form as well
as providing the user with adequate feedback that the page is doing something.
Disabling the button prevents the form being submitted twice and the error
feedback should hopefully offer a solution for the error that occurred.

### Event Handlers

When using event handlers to listen for browser events it's a common
requirement to want to cancel the default browser action. This should be
done by calling the `event.preventDefault()` method:

    jQuery('button').click(function (event) {
      event.preventDefault();
    });

It is also possible to return `false` from the callback function. Avoid doing
this as it also calls the `event.stopPropagation()` method which prevents the
event from bubbling up the DOM tree. This prevents other handlers listening
for the same event. For example an analytics click handler attached to the
`<body>` element.

Also jQuery (1.7+) now provides the `.on()` and `.off()` methods as
alternatives to `.bind()`, `.unbind()`, `.delegate()` and `.undelegate()`
and they should be preferred for all tasks.


### Closures

_TODO_

### Templating

_TODO_

Resources
---------

_TODO_

{:js: data-code="js"}
