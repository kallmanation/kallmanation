## How to use Vercel's serverless functions to make an ipify clone

[Vercel](https://vercel.com/) (formerly [ZEIT now](https://zeit.co)) is a fantastic platform for deploying static websites and serverless functions. Let's dig into what their serverless functions can do by rebuilding [ipify](https://www.ipify.org/) in node.js (which if you didn't know is a fantastic and totally free reverse ip address lookup service).

Prerequisites
---
I'm assuming you've gotten an account with [Vercel](https://vercel.com/signup) and installed their [cli](https://vercel.com/download).
You will also need [npm](https://www.npmjs.com/get-npm) as we will be using the [request-ip](https://www.npmjs.com/package/request-ip) package to handle finding the IP out of the HTTP request.

Project Setup
---

Anywhere on your computer, let's create a folder called `ipify-clone` for our project:
```bash
$ mkdir ipify-clone
$ cd ipify-clone
```

And let's put the basic folders and files we'll need in there:
```bash
$ echo '{}' > package.json
$ echo '{ "version": 2 }' > now.json
$ echo 'ipify clone' > index.html
$ mkdir 'api'
$ touch 'api/json.js'
$ touch 'api/text.js'
$ mkdir 'api/jsonp'
$ touch 'api/jsonp/[callback].js'
```

And now let's deploy once to confirm everything is working properly:
```
$ now
```

After going through any prompts that gives you and allowing it to finish, it should copy the url it deployed to into your pasteboard (it should also be displayed in the command output). Open a browser and visit that link; you should see a page that says `ipify clone` in the top left corner. Not the most exciting webpage in the world but we have to start somewhere right?

Rebuilding ipify
---

There's three endpoints we're going to build:
1. Return the ip as plain text
2. Return the ip in json, like `{ "ip": "255.255.255.255" }`
3. Return the ip in [jsonp](https://stackoverflow.com/questions/2067472/what-is-jsonp-and-why-was-it-created) with a custom callback, like `userSuppliedCallback({ "ip": "255.255.255.255" })`

For all of them, we'll use the [request-ip](https://www.npmjs.com/package/request-ip) package to actually get the IP address. So, let's install that and start making the first endpoint:
```bash
$ npm install request-ip --save
```

Building the text api
---

In your favorite text editor, open the JavaScript file we created at `ipify-clone/api/text.js`. We want to do three things for this endpoint:
1. Set the HTTP status to `200` (OK)
2. Set the `Content-Type` header to `text/plain` to tell everyone this is a plaintext response
3. Get the IP out of the request and set it as the only body of our response

The code for that looks like this:
```js
const requestIp = require('request-ip');

module.exports = (req, res) => {
  res.setHeader('Content-Type', 'text/plain');
  res.status(200).send(requestIp.getClientIp(req));
}
```

Since this is our first endpoint, let's go into detail for this one. 

First, we need to include that package we installed so we can use it, so `const requestIp = require('request-ip');`. 

Then, the way Vercel works is we need to set `module.exports` to an arrow function, that accepts two objects: the request and the response; `(req, res) => { ... }`. This will be the same for all of our serverless functions.

Inside the function, our job is to manipulate the response object using the request object to make our api do what we want. `.setHeader` is how we set the `Content-Type` header we want; `.status` is how we set our status; `requestIp.getClientIp` is how we get the IP address; and `.send` is how we set the body of our response.

Let's deploy again and see what happens:
```bash
$ now
```

Again taking the url it gives us let's visit `<the-deployment-url>/api/text`.

If all worked you should see your IP address! Notice how Vercel took our `text.js` file in the `api` directory and turned it into an endpoint located at `/api/text` (and if you inspect the page and look at the request, you should see the headers include `text/plain`). Vercel does this automatically for any file or folder located in `/api`.

One down, two endpoints to go!

Building the json api
---
This is almost exactly the same as the text endpoint; the only differences are:
1. We want to set the `Content-Type` header to `application/json` instead of `text/plain`
2. Wrap the IP in a JSON object when returning

Vercel has a nice method off the response object for returning JSON, named (creatively) `.json`. Otherwise the code to put in the `ipify-clone/api/json.js` file should look familiar:
```js
const requestIp = require('request-ip');

module.exports = (req, res) => {
  res.setHeader('Content-Type', 'application/json');
  res.status(200).json({ ip: requestIp.getClientIp(req) });
}
```

If you deploy again and visit `<the-deployment-url>/api/json`, you should again see your IP, but this time wrapped in JSON! I know it's a big accomplishment but try to contain your excitement.

(We could have also just built the json return manually)
```js
const requestIp = require('request-ip');

module.exports = (req, res) => {
  res.setHeader('Content-Type', 'application/json');
  res.status(200).send(`{ "ip": ${requestIp.getClientIp(req)} }`);
}
```

Building the jsonp api
---

For this endpoint, we'd like to allow the client to specify what callback to use in the [jsonp](https://stackoverflow.com/questions/2067472/what-is-jsonp-and-why-was-it-created). There's a lot of ways this could be done, but let's use Vercel's [path segments](https://vercel.com/docs/v2/serverless-functions/introduction#path-segments) to demonstrate what they can do.

If we name a file in our api directory with square brackets, like `[parameter].js`, Vercel will allow any request like `api/anything` or `api/somethingelse` and call that `[parameter].js` function with the value as a parameter in the request object.

So, by making a function in `ipify-clone/api/jsonp/[callback].js` our users will be able to visit `/api/jsonp/customCallback` and we can include that value of `customCallback` in our response by accessing `req.query.callback`.

```js
const requestIp = require('request-ip');

module.exports = (req, res) => {
  res.setHeader('Content-Type', 'application/javascript');
  res.status(200).send(`${req.query.callback}({"ip": "${requestIp.getClientIp(req)}"})`);
}
```

Deploy again and visit `<the-deployment-url>/api/jsonp/callback` and you should get a response like `callback({"ip": "255.255.255.255"})`. And of course you can visit other paths like `<the-deployment-url>/api/jsonp/customCallback` or whatever you'd like.

Wrapping up
---

You can deploy this to production using `now --prod`. If you've purchased a domain, you can alias to that using the Vercel dashboard. Checkout my deployment at https://ripal.klmntn.com/.
 - Text: https://ripal.klmntn.com/api/text
 - JSON: https://ripal.klmntn.com/api/json
 - JSONP: https://ripal.klmntn.com/api/jsonp/callback

%[https://github.com/kallmanation/ripal]