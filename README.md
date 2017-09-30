# modfun :cloud:

Moderate fun with Modular Functions: a fast no-dependencies function router for cloud functions.

```js
var modfun = require('modfun')

var controller = {
  getUser = function(req, res) {
    ...
    res.status(200).json(user)
  }
}

var app = modfun(controller)
```

```
GET http://cloudfunction/project/getUser/[username]
```

Enhance with middleware and custom error handlers:

```js
var modfun = require('modfun')
var morgan = require('morgan')
var cors = require('cors')
var jwt = require('express-jwt')

...

exports.app = modfun(
  {
    authenticate: authenticate,
    user: [authorize, getUser] // middleware preceding operation
  },
  {
    middleware: [logger], // global middleware that runs every time
    errorHandler: (err, req, res) => res.status(500).send(err.message) // custom error handler
  }
)
```

Easy to expose an existing module to Google Cloud Functions:

user-module.js
```js
exports.get = (username) => {
  ...
  return user;
}
exports.giveProps = (username) => {
  ...
  return;
}
```

index.js
```js
var modfun = require('modfun')
var myModule = require('./user-module')

exports.user = modfun(myModule, { mode: 'function' })
```

And apply commonly used middleware:

```js
var modfun = require('modfun')
var morgan = require('morgan')
var cors = require('cors')
var jwt = require('express-jwt')
var controller = require('./service-controller')

exports.service = modfun(controller, [ morgan('tiny'), cors(), jwt(secret) ])
```

## Features
  * Basic routing to functions
  * Parameter parsing
  * Automatic HTTP reponse building
  * Connect/Express-like middleware support
  * Google Cloud Functions
  * [Future] AWS Lambda
  * Can act as a middleware on other Connect-based frameworks

## Installation

```bash
$ npm install modfun
```

Or

```bash
$ yarn add modfun
```
