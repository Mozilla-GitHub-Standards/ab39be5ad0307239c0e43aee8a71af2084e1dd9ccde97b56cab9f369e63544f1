# Bugzilla Readable Status Messages 

[![Build Status](https://travis-ci.org/mozilla/bugzilla-readable-status.svg?branch=master)](https://travis-ci.org/mozilla/bugzilla-readable-status)

Generate readable, english language, status messages from properties of bugs in bugzilla.mozilla.org.

The module exposes a single method, `readable,` which can be called in a variety of ways:

* with an object of properties corresponding to a bug's properties.
* the id of a bug (to be implemented)
* an array of objects containing bug properties
* an array of bug ids (to be implemented)

## Install

Clone repo to local.

`npm install; npm test; npm run demo`

### For browsers

Install browserify

`npm install -g browserify`

Run the project's bundle script.

`npm run bundle`

And load the resulting file into your web page.

```
<script src="bugzilla-readable-status.js"></script>
```

To build a minified version of the script, 

`npm run bundle-min` 

and then include

```
<script src="bugzilla-readable-status-min.js"></script>
```

in your project. A corresponding source map is generated as well.

## Usage

```
var readable = import('bugzilla-readable-status').readable;

var status = readable({ id: NNNNNN, status: 'NEW', … });

// => 'BUG STATUS'

var statuses = readable([
        { id: NNNNNN, status: 'FIXED' … }.
        { id: MMMMMM, status: 'NEW' … },
        …
    ]);

// => { "statuses": [
        { "id": NNNNNN, "status": "BUG STATUS" },
        { "id": MMMMMM, "status": "BUG STATUS" },
        …
    ]}

var emptyArray = readable([]);

// => { "result": [] }
```

## Errors

Errors are returned as objects:

```
{ "error": "ERROR STRING" }
```

### Unable to Parse

If there's an unrecoverable error while trying to parse a bug, return `CANNOT_PARSE_BUG`.

### No Data

If there are no usable properties or the module can't generate, `NO_STRING_FOR_BUG`

### Errors in Requests for Multiple Bugs 

If multiple bugs are requested, and errors are found, the errors are included in the reponse object:

```
{ "statuses": [
        { "id": "BUG ID", 
          "status": "BUG STATUS" },
        …
        { "id": "BUG ID WITH ERROR",
          "error": "ERROR MESSAGE" }
        …
    ]}
```

