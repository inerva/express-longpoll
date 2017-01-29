# express-longpoll

Lightweight long polling module for express.js

### Description

Sets up basic long poll with subscriibe and publish functionality.

### Install

```$ npm install -S express-longpoll ```

### Usage

**Quick-start code**  
```
var express = require('express');
var app = express();
var longpoll = require("express-longpoll")(app);

// Creates app.get("/poll") for the long poll
longpoll.create("/poll");

app.listen(8080, function() {
    console.log("Listening on port 8080");
});

var data = { text: "Some data" };

// Publishes data to all clients long polling /poll endpoint
longpoll.publish("/poll", data);
```

###**longpoll.create(url, [options])**  
  Sets up an express endpoint using the URL provided.

```
var express = require('express');
var app = express();
var longpoll = require("express-longpoll")(app);

longpoll.create("/poll");
longpoll.create("/poll2", { maxListeners: 100 }); // set max listeners
```

###**longpoll.publish(url, data)**  
  Publishes ```data``` to all listeners on the ```url``` provided.

```
var express = require('express');
var app = express();
var longpoll = require("express-longpoll")(app);

longpoll.create("/poll");

longpoll.publish("/poll", {});
longpoll.publish("/poll", { hello: "Hello World!" });
longpoll.publish("/poll", jsonData);
```

## Using Promises

```
longpoll.create("/poll")
  .then(() => {
    console.log("Created /poll");
  })
  .catch((err) => {
    console.log("Something went wrong!", err);
  });
  
longpoll.publish("/poll", data)
  .then(() => {
    console.log("Published to /poll:", data);
  })
  .catch((err) => {
    console.log("Something went wrong!", err);
  });
```

## Sample clientside code to subscribe to the longpoll

###**Client using jQuery**
```
var subscribe = function(url, cb) {
        $.ajax({
            method: 'GET',
            url: url,
            success: function(data) {
                cb(data);
            },
            complete: function() {
                setTimeout(function() {
                    subscribe(url, cb);
                }, 1000);
            },
            timeout: 30000
        });
    };
    
subscribe("/poll", function (data) {
  console.log("Data:", data);
});

subscribe("/poll2", function (data) {
  console.log("Data:", data);
});
```
