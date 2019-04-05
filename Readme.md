An complete executable example here at: https://github.com/waychan23/koa2-oauth-server/tree/master/examples/complete-example.

This repository is forked from [express-oauth-server](https://github.com/oauthjs/express-oauth-server). The reason why I choose [express-oauth-server](https://github.com/oauthjs/express-oauth-server) over [koa-oauth-server](https://github.com/oauthjs/koa-oauth-server) is the latter seems to be a little out of maintenance.

Complete, compliant and well tested module for implementing an OAuth2 Server/Provider with [koa2](https://github.com/koajs/koa) in [node.js](http://nodejs.org/).

This is the koa2 wrapper for [oauth2-server](https://github.com/oauthjs/node-oauth2-server).

## Installation

    $ npm install @authing/koa2-oauth-server

## Quick Start

The module provides two middlewares - one for granting tokens and another to authorize them. `koa2-oauth-server` and, consequently `oauth2-server`, expect the request body to be parsed already.
The following example uses `koa-bodyparser` but you may opt for an alternative library.

```js
var bodyParser = require('koa-bodyparser');
var koa = require('koa');
var OAuthServer = require('koa2-oauth-server');

var app = new koa();

app.oauth = new OAuthServer({
  model: {}, // See https://github.com/oauthjs/node-oauth2-server for specification
});

app.use(bodyParser());
app.use(app.oauth.authorize());

app.use(async (ctx, next) => {
  ctx.body = 'Secret area';
  await next();
});

app.listen(3000);
```

## Options

```
var options = { 
  useErrorHandler: false, 
  continueMiddleware: false,
}
```
* `useErrorHandler`
(_type: boolean_ default: false)

  If false, an error response will be rendered by this component.
  Set this value to true to allow your own error handler to handle the error.
  
  **Note:** the `error` object will be passed along the middleware chain as `ctx.state.oauth.error`.

* `continueMiddleware`
(_type: boolean default: false_)

  The `authorize()` and `token()` middlewares will both render their 
  result to the response and end the pipeline.
  next() will only be called if this is set to true.

  **Note:** You cannot modify the response since the headers have already been sent.

  `authenticate()` does not modify the response and will always call next()
