---
layout: post
title: "Notes on RESTing with AngularJS and ExpressJS"
published: true
---

REST, or Representational State Transfer, is a stateless communications protocol for server-client communication. Again in English, REST is a way of designing your Application Programming Interface so that both the server and client can exchange messages without having to remember past requests. This post is a short note on how to send and receive messages from/to [AngularJS](https://angularjs.org/) from/to [ExpressJS](http://expressjs.com/) (Node).

---

The HTTP protocol specifies 4 primary verbs - POST, GET, PUT, DELETE corresponding to the basic Create, Read, Update, Delete (CRUD) operations. Both AngularJS and ExpressJS implement it but the difficult part to remember lies in the differences in how the data is received and how to extract the data for use.

Before we begin, all `router`s are instantiated as such:

```javascript
var router = require('express').Router();
```

The `router`s can be used by making your primary app object use it:

```javascript
var app = express();
var router = express.Router();
app.use(router);
```

`router`s can also use other instances of themselves and it is a great way to structure your API so that each file can represent an endpoint.

```javascript
var router = require('express').Router();
var router2 = require('express').Router();
router.use('/router2', router2);
```

## GET
`GET`ting is the most straight forward. There are two ways you can send arguments, one is via the URL, the other is via a URL query parameter (the text after the question mark). Always remember to escape your queries!

#### AngularJS
```javascript
var endpoint = '/api/endpoint/1';
var dataToSend = 'query_type' + 
    encodeURIComponent('Hello World');

$http.get(
    endpoint + '?' + dataToSend
).then(function(response) {
    console.log(response);
});
```
#### ExpressJS
```javascript
router.get(
    '/api/endpoint/:id', 
    function(req, res, next) {
        res.json({
            req_type: 'get',
            id: req.params.id
            query_type: req.query.query_type
        });
    }
);
```

## POST
`POST`ing can be slightly tricky and you'll need to `npm install` the `body-parser` module, require it and use it before you can read information from `POST` HTTP requests.


#### AngularJS
```javascript
var endpoint = '/api/endpoint/1';
var dataToSend = { 
    queryType: 'Hello World' 
};

$http.post(endpoint, dataToSend)
    .then(function(response) {
        console.log(response);
    });
```

#### ExpressJS
```javascript
router.post(
    '/api/endpoint/:id',
    function(req, res, next) {
        res.json({
            req_type: 'post',
            id: req.params.id
            query_type: req.body.queryType
        });
    }
);
```

## PUT
Similar to `POST` requests, `PUT`ting also requires the `body-parser` module.

#### AngularJS
```javascript
var endpoint = '/api/endpoint/1';
var dataToSend = { 
    queryType: 'Hello World' 
};

$http.post(endpoint, dataToSend)
    .then(function(response) {
        console.log(response);
    });
```

#### ExpressJS
```javascript
router.put(
    '/api/endpoint/:id',
    function(req, res, next) {
        res.json({
            req_type: 'put',
            id: req.params.id
            query_type: req.body.queryType
        });
    }
);
```

## DELETE
`DELETE` and `GET` share the mechanism. Like `GET`, the `DELETE` method does not have a body segment. Instead, parameters are passed in the URL or as a URL query parameter.

#### AngularJS
```javascript
var endpoint = '/api/endpoint/1';
var dataToSend = 'query_type' + 
    encodeURIComponent('Hello World');

$http.delete(
    endpoint + '?' + dataToSend
).then(function(response) {
    console.log(response);
});
```
#### ExpressJS
```javascript
router.delete(
    '/api/endpoint/:id', 
    function(req, res, next) {
        res.json({
            req_type: 'delete',
            id: req.params.id
            query_type: req.query.query_type
        });
    }
);
```

Hope this helps someone somewhere out there! Till the next time-
