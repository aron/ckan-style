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

_TODO_

Documentation
-------------

_TODO_

Testing
-------

_TODO_

Best Practices
--------------

### Forms

_TODO_

### Ajax

_TODO_

### Closures

_TODO_

### Templating

_TODO_

Resources
---------

_TODO_

{:js: data-code="js"}
