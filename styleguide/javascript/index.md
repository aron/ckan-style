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

_NOTE: See section 2.D.1.1 of idiomatic for more examples of this syntax._

### Quotes

Single quotes should be used everywhere unless writing JSON or the string
contains them. This makes it easier to create strings containing HTML.

    jQuery('<div id="my-div" />').appendTo('body');

Object properties need not be quoted unless required by the compiler.

    var object = {
      name: 'bill',
      'class': 'user-name'
    };

### Variable declarations

Idiomatic recommends using only one `var` statement per function. This is good
advice but can be impractical in [some situations][#var], so one `var`
statement may be used but it will not be enforced.

    // GOOD:
    var good = "string",
        alsoGood = "another;

    // BAD:
    var good = "string",
        iffy = [
      "hmm", "not", "great"
    ];

    // BETTER:
    var good = "string";
    var okay = [
      "hmm", "a bit", "better"
    ];

    // ALTERNATIVELY
    var good, okay;

    good = "string";
    okay = [
      "hmm", "also", "better"
    ];

However all variables must be declared at the top of the function in which
they are first used.

    // BAD: contrived example.
    function lowercaseNames(names) {
      var names = [];

      for (var index = 0, length = names.length; index < length; index += 1) {
        names.push(names[index].toLowerCase());
      }

      var sorted = names.sort();
      return sorted;
    }

    // GOOD:
    function lowercaseNames(names) {
      var names = [], index, sorted;

      for (index = 0, length = names.length; index < length; index += 1) {
        names.push(names[index].toLowerCase());
      }

      sorted = names.sort();
      return sorted;
    }

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

Constructor functions must use uppercase CamelCase:

    function DatasetSearchView() {
    }

Constants must be uppercase with spaces delimited by underscores:

    var env = {
      PRODUCTION:  'production',
      DEVELOPMENT: 'development',
      TESTING:     'testing'
    };

Event handlers and callback functions should be prefixed with "on":

    function onDownloadClick(event) {}
    jQuery('.download').click(onDownloadClick);

Boolean variables or methods returning boolean functions should prefix
the variable name with "is":

    function isAdmin() {}

    var canEdit = isUser() && isAdmin();

_Alternatives are "has", "can" and "should" if they make more sense_

Private methods should be prefixed with an underscore:

    View.extend({
      "click": "_onClick",
      _onClick: function (event) {
      }
    });

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
