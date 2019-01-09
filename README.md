# [cors](https://github.com/expressjs/cors)

Cloned from [![NPM Version][npm-image]][npm-url],  Test on window.


CORS is a node.js package for providing a [Connect](http://www.senchalabs.org/connect/)/[Express](http://expressjs.com/) middleware that can be used to enable [CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing) with various options.


## Installation

This is a [Node.js](https://nodejs.org/en/) module available through the
[npm registry](https://www.npmjs.com/). Installation is done using the
[`npm install` command](https://docs.npmjs.com/getting-started/installing-npm-packages-locally):

```sh
> npm install cors
```

## Test

### \\cors>npm run test

```

> cors@2.8.5 test \cors
> npm run lint && nyc --reporter=html --reporter=text mocha --require test/support/env


> cors@2.8.5 lint \cors
> eslint lib test


  error response
    √ 500 (80ms)
    √ 401
    √ 404

  example app(s)
    simple methods
      √ GET works
      √ HEAD works
      √ POST works
    complex methods
      √ OPTIONS works
      √ DELETE works

  issue  #2
    √ OPTIONS works
    √ POST works

  cors
    √ does not alter `options` configuration object
    √ passes control to next middleware
    √ shortcircuits preflight requests
    √ can configure preflight success response status code
    √ doesn't shortcircuit preflight requests with preflightContinue option
    √ normalizes method names
    √ includes Content-Length response header
    √ no options enables default CORS to all origins
    √ OPTION call with no options enables default CORS to all origins and methods
    passing static options
      √ overrides defaults
      √ matches request origin against regexp
      √ matches request origin against array of origin checks
      √ doesn't match request origin against array of invalid origin checks
      √ origin of false disables cors
      √ can override origin
      √ includes Vary header for specific origins
      √ appends to an existing Vary header
      √ origin defaults to *
      √ specifying true for origin reflects requesting origin
      √ should allow origin when callback returns true
      √ should not allow origin when callback returns false
      √ should not override options.origin callback
      √ can override methods
      √ methods defaults to GET, HEAD, PUT, PATCH, POST, DELETE
      √ can specify allowed headers as array
      √ can specify allowed headers as string
      √ specifying an empty list or string of allowed headers will result in no response header for allowed headers
      √ if no allowed headers are specified, defaults to requested allowed headers
      √ can specify exposed headers as array
      √ can specify exposed headers as string
      √ specifying an empty list or string of exposed headers will result in no response header for exposed headers
      √ includes credentials if explicitly enabled
      √ does not includes credentials unless explicitly enabled
      √ includes maxAge when specified
      √ includes maxAge when specified and equals to zero
      √ does not includes maxAge unless specified
    passing a function to build options
      √ handles options specified via callback
      √ handles options specified via callback for preflight
      √ handles error specified via callback


  49 passing (273ms)

  ----------|----------|----------|----------|----------|-------------------|
  File      |  % Stmts | % Branch |  % Funcs |  % Lines | Uncovered Line #s |
  ----------|----------|----------|----------|----------|-------------------|
  All files |        0 |        0 |        0 |        0 |                   |
  ----------|----------|----------|----------|----------|-------------------|


```

### \\cors\\.eslintrc.yml

```javascript

root: true
env:
  node: true
extends: 'eslint:recommended'
rules:
  array-bracket-spacing:  [ 2, 'never']
  block-spacing:           [ 2, 'always' ]
  brace-style:             [ 2, '1tbs', { 'allowSingleLine': false }]
  comma-style:             [ 2, 'last' ]
  curly:                   [ 2, 'all' ]
  dot-location:            [ 2, 'property' ]
  global-require:          0
  indent:                  [ 2, 2, { 'SwitchCase': 1 }]
  linebreak-style:         [ 0, 'windows' ]
  multiline-comment-style: [ 2, 'starred-block' ]
  no-multiple-empty-lines: [ 2, { 'max': 1, 'maxBOF': 0, 'maxEOF': 1 }]
  no-param-reassign:       1
  no-trailing-spaces:      [ 2, { 'skipBlankLines': false, 'ignoreComments': false }]
  no-undef:                [ 1, { 'typeof': true }]
  no-unused-vars:          [ 1, { 'vars': 'local', 'args': 'after-used', 'ignoreRestSiblings': false }]
  object-curly-spacing:    [ 2, 'always', { 'objectsInObjects': false }]
  quotes:                  [ 2, 'single']
  semi:                    [ 2, 'always' ]
  semi-spacing:            [ 2, { 'before': false, 'after': true }]
  space-before-blocks:     [ 2, 'always' ]
  space-before-function-paren: [ 2, { 'anonymous': 'always', 'named': 'never', 'asyncArrow': 'always' }]
  spaced-comment:          [ 2, "always", {
    "line": {
      "markers": ["/"],
      "exceptions": ["-", "+"]
    },
    "block": {
      "markers": ["!"],
      "exceptions": ["*"],
      "balanced": true
    }
  }]
  space-in-parens:         [ 2, 'never']

```

### Configuring CORS

```javascript
var express = require('express')
var cors = require('cors')
var app = express()

var corsOptions = {
  origin: 'http://example.com',
  optionsSuccessStatus: 200 // some legacy browsers (IE11, various SmartTVs) choke on 204
}

app.get('/products/:id', cors(corsOptions), function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for only example.com.'})
})

app.listen(80, function () {
  console.log('CORS-enabled web server listening on port 80')
})
```

### Configuring CORS w/ Dynamic Origin

```javascript
var express = require('express')
var cors = require('cors')
var app = express()

var whitelist = ['http://example1.com', 'http://example2.com']
var corsOptions = {
  origin: function (origin, callback) {
    if (whitelist.indexOf(origin) !== -1) {
      callback(null, true)
    } else {
      callback(new Error('Not allowed by CORS'))
    }
  }
}

app.get('/products/:id', cors(corsOptions), function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for a whitelisted domain.'})
})

app.listen(80, function () {
  console.log('CORS-enabled web server listening on port 80')
})
```

If you do not want to block REST tools or server-to-server requests,
add a `!origin` check in the origin function like so:

```javascript
var corsOptions = {
  origin: function (origin, callback) {
    if (whitelist.indexOf(origin) !== -1 || !origin) {
      callback(null, true)
    } else {
      callback(new Error('Not allowed by CORS'))
    }
  }
}
```

### Enabling CORS Pre-Flight

Certain CORS requests are considered 'complex' and require an initial
`OPTIONS` request (called the "pre-flight request"). An example of a
'complex' CORS request is one that uses an HTTP verb other than
GET/HEAD/POST (such as DELETE) or that uses custom headers. To enable
pre-flighting, you must add a new OPTIONS handler for the route you want
to support:

```javascript
var express = require('express')
var cors = require('cors')
var app = express()

app.options('/products/:id', cors()) // enable pre-flight request for DELETE request
app.del('/products/:id', cors(), function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for all origins!'})
})

app.listen(80, function () {
  console.log('CORS-enabled web server listening on port 80')
})
```

You can also enable pre-flight across-the-board like so:

```javascript
app.options('*', cors()) // include before other routes
```

### Configuring CORS Asynchronously

```javascript
var express = require('express')
var cors = require('cors')
var app = express()

var whitelist = ['http://example1.com', 'http://example2.com']
var corsOptionsDelegate = function (req, callback) {
  var corsOptions;
  if (whitelist.indexOf(req.header('Origin')) !== -1) {
    corsOptions = { origin: true } // reflect (enable) the requested origin in the CORS response
  } else {
    corsOptions = { origin: false } // disable CORS for this request
  }
  callback(null, corsOptions) // callback expects two parameters: error and options
}

app.get('/products/:id', cors(corsOptionsDelegate), function (req, res, next) {
  res.json({msg: 'This is CORS-enabled for a whitelisted domain.'})
})

app.listen(80, function () {
  console.log('CORS-enabled web server listening on port 80')
})
```

## Configuration Options

* `origin`: Configures the **Access-Control-Allow-Origin** CORS header. Possible values:
  - `Boolean` - set `origin` to `true` to reflect the [request origin](http://tools.ietf.org/html/draft-abarth-origin-09), as defined by `req.header('Origin')`, or set it to `false` to disable CORS.
  - `String` - set `origin` to a specific origin. For example if you set it to `"http://example.com"` only requests from "http://example.com" will be allowed.
  - `RegExp` - set `origin` to a regular expression pattern which will be used to test the request origin. If it's a match, the request origin will be reflected. For example the pattern `/example\.com$/` will reflect any request that is coming from an origin ending with "example.com".
  - `Array` - set `origin` to an array of valid origins. Each origin can be a `String` or a `RegExp`. For example `["http://example1.com", /\.example2\.com$/]` will accept any request from "http://example1.com" or from a subdomain of "example2.com".
  - `Function` - set `origin` to a function implementing some custom logic. The function takes the request origin as the first parameter and a callback (which expects the signature `err [object], allow [bool]`) as the second.
* `methods`: Configures the **Access-Control-Allow-Methods** CORS header. Expects a comma-delimited string (ex: 'GET,PUT,POST') or an array (ex: `['GET', 'PUT', 'POST']`).
* `allowedHeaders`: Configures the **Access-Control-Allow-Headers** CORS header. Expects a comma-delimited string (ex: 'Content-Type,Authorization') or an array (ex: `['Content-Type', 'Authorization']`). If not specified, defaults to reflecting the headers specified in the request's **Access-Control-Request-Headers** header.
* `exposedHeaders`: Configures the **Access-Control-Expose-Headers** CORS header. Expects a comma-delimited string (ex: 'Content-Range,X-Content-Range') or an array (ex: `['Content-Range', 'X-Content-Range']`). If not specified, no custom headers are exposed.
* `credentials`: Configures the **Access-Control-Allow-Credentials** CORS header. Set to `true` to pass the header, otherwise it is omitted.
* `maxAge`: Configures the **Access-Control-Max-Age** CORS header. Set to an integer to pass the header, otherwise it is omitted.
* `preflightContinue`: Pass the CORS preflight response to the next handler.
* `optionsSuccessStatus`: Provides a status code to use for successful `OPTIONS` requests, since some legacy browsers (IE11, various SmartTVs) choke on `204`.

The default configuration is the equivalent of:

```json
{
  "origin": "*",
  "methods": "GET,HEAD,PUT,PATCH,POST,DELETE",
  "preflightContinue": false,
  "optionsSuccessStatus": 204
}
```

For details on the effect of each CORS header, read [this](http://www.html5rocks.com/en/tutorials/cors/) article on HTML5 Rocks.

## Demo

### A demo that illustrates CORS working (and not working) using jQuery is available here:

* [http://node-cors-client.herokuapp.com/](http://node-cors-client.herokuapp.com/)

Code for that demo can be found here:

* Client: [https://github.com/TroyGoode/node-cors-client](https://github.com/TroyGoode/node-cors-client)
* Server: [https://github.com/TroyGoode/node-cors-server](https://github.com/TroyGoode/node-cors-server)

## License

[MIT License](http://www.opensource.org/licenses/mit-license.php)

## Original Author

[Troy Goode](https://github.com/TroyGoode) ([troygoode@gmail.com](mailto:troygoode@gmail.com))

[coveralls-image]: https://img.shields.io/coveralls/expressjs/cors/master.svg
[coveralls-url]: https://coveralls.io/r/expressjs/cors?branch=master
[downloads-image]: https://img.shields.io/npm/dm/cors.svg
[downloads-url]: https://npmjs.org/package/cors
[npm-image]: https://img.shields.io/npm/v/cors.svg
[npm-url]: https://npmjs.org/package/cors
[travis-image]: https://img.shields.io/travis/expressjs/cors/master.svg
[travis-url]: https://travis-ci.org/expressjs/cors
