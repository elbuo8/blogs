# JSON Web Tokens(again!) and Koa.js

A few weeks ago I wrote about using JSON Web Tokens (JWT) has a alternative method of authentication using Go. You can find that post [here](sendgrid.com/blog/tokens-tokens-intro-json-web-tokens-jwt-go/). The post itself contains a basic explanation about JWT and some links for content. In this post, I want to cover pretty much the same things using [Koa](http://koajs.com), a new framework for Node.js.

Koa is mostly a  middleware framework which uses generators (only available under the Harmony flag) to control the flow of requests. The reason I mention its a middleware framework is because it doesn't come with most of the things you would expect a framework to provide such as routing, body parsers, etc. But when it comes to middleware, Koa is baller.

<center>![BATMOBIL](http://g.purevolumecdn.com/cdnImages/crop_345x235/Artist-2296722-qttb.jpg)</center>

## To The Code!

First install the following packages:

```bash
$ npm install koa koa-jwt co-body --save
```

This tutorial will be using RSA keys instead of simple strings you have a better sense of what you could use in production. Generating your keys its as simple as running the following commands:

```bash
$ openssl genrsa -out demo.rsa 1024 # the 1024 is the size of the key we are generating
$ openssl rsa -in demo.rsa -pubout > demo.rsa.pub
```

Now that we have all that we need, lets get some application code done.

```js
var koa = require('koa');
var parse = require('co-body');
var jwt = require('koa-jwt');
var fs = require('fs');

var app = koa();
var publicKey = fs.readFileSync('demo.rsa.pub');
var privateKey = fs.readFileSync('demo.rsa');

// JWT Error Catcher
app.use(function *(next) {
  try {
    yield next; //Attempt to go through the JWT Validator
  } catch(e) {
    if (e.status == 401 ) {
      // Prepare response to user.
      this.status = e.status;
      this.body = 'You don\'t have a signed token dude :('
    } else {
      throw e; // Pass the error to the next handler since it wasn't a JWT error.
    }
  }
});

// Public endpoint to login.
app.use(function *(next) {
  if (this.url.match(/^\/login/)) {
    var claims = yield parse(this);
    var token = jwt.sign(claims, privateKey, {algorithm: 'RS256'});
    this.status = 200;
    this.body = {token: token};
  } else {
    yield next;
  }
});

// Everything behind this will be protected.
app.use(jwt({
  secret: publicKey,
  algorithm: 'RS256'
}));

app.use(function *() {
  this.status = 200;
  this.body = 'You are logged in dude! Welcome!';
});

app.listen(3000);
```

Lets go step by step. Initially we just set up Koa normally and then we read our previously created public and private keys which we will use to sign and verify the tokens.

Notice that the very first middleware is simply used to catch the errors that JWT in order to obfuscate the original errors. Followed by that, there is a public endpoint where users go to obtain their tokens. Koa's middlewares get executed in the order they are added to the pipeline so everything that gets added after the introduction of the JWT middleware will require a valid signed token.

Lets test this out!

```bash
$ curl localhost:3000/api
You don't have a signed token dude :(
$ curl -X POST -H "Content-Type: application/json" localhost:3000/login -d '{"username": "elbuo8"}'
{"token": "verylongtokenstring :)"}
$ curl -X POST -H "Authorization: Bearer verylongtokenstring :)" localhost:3000/api -d '{"username": "elbuo8"}'
You are logged in dude! Welcome!
```

Hope this tutorial was useful and entertaining! If you have any questions, doubts or concerns feel free to ping me! [@elbuo8](https://twitter.com/elbuo8)
