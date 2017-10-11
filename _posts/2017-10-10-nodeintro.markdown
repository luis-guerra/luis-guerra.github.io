---
layout: post
title:  "NodeJS Test"
date:   2017-10-10 17::02 +0200
---

I created a very simple node.js app that opens up a web server on my local port 8080:

```javascript
let http = require('http');

http.createServer( function (req, res){

  res.writeHead(200,{'Content-Type':'text/html'});

  res.write('<h1> Node.js works!! <h1>');

  res.end();

}).listen(8080);
```
