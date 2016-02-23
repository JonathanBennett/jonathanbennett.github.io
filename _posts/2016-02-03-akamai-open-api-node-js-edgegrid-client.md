---
layout: article
title: Akamai {OPEN} API Node.js EdgeGrid Client
modified:
categories:
excerpt:
tags: []
image:
  teaser: 1x1.gif
  thumb: 1x1.gif
date: 2014-06-22T22:06:45+00:00
comments: true
---

Today I published version 1.0.0 of the Akamai {OPEN} EdgeGrid Node.js EdgeGrid client that follows their (comprehensive...) authentication scheme to help Node.js developers execute against their API. You can head on over to [here](https://github.com/JonathanBennett/AkamaiOPEN-edgegrid-node) to fork the code.

**UPDATE**: I have now released version 1.1.0 after a PR from [@dariusk](https://github.com/dariusk) (Thanks!). It introduces a breaking change if you picked up 1.0.0. The example has been updated below.

After long dillema over how the calling syntax should work, I settled on chaining being the most effective and easiest to use way to use the SDK.

Example:

```javascript
var EdgeGrid = require('edgegrid');

var client_token = "akab-access-token-xxx-xxxxxxxxxxxxxxxx",
  client_secret = "akab-client-token-xxx-xxxxxxxxxxxxxxxx",
  access_token = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx=",
  base_uri = "https://akaa-baseurl-xxxxxxxxxxx-xxxxxxxxxxxxx.luna.akamaiapis.net/";

var data = "datadatadatadatadatadatadatadata";

var eg = new EdgeGrid(client_token, client_secret, access_token, base_uri);

eg.auth({
  "url": "billing-usage/v1/products",
  "method": "POST",
  "headers": {},
  "body": data
}).send(function (data, response) {
  console.log(data);
});
```

If you found this useful feel free to let me know!
