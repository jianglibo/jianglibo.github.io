---
layout: post
title:  "Share Copy Paster content between all subscribes"
date:   2021-11-19 16:27:04 +0300
description: Share text content between hosts, if remote controll doesn't have copy paster support.
categories: java
custom_js:
- https://js.pusher.com/7.0/pusher.min.js
- pusher.elm
custom_css:
- multiple-line-escape

---


<div id="myapp"><div id="elmapp"></div></div>
<script>
    console.log("hash: " + window.location.hash);
    var app_host = "https://cpsyc.resp.me";
    var app = Elm.Pusher.init({
      node: document.getElementById('elmapp'),
      flags: { 
        cpsyc_url: app_host,
        channel_name: window.location.hash 
       }
    });

    app.ports.subscribeToPusher.subscribe(function (channel_name) {
      window.location.hash = '#' + channel_name;
      console.log("about to subscribe to " + channel_name);
      var pusher = new Pusher('83301c9b5a9e5ad71ee6', {
        cluster: 'eu',
        authEndpoint: app_host + "/channel/auth",
        authTransport: "ajax"
      });

      var socketId = null;
      pusher.connection.bind("connected", () => {
        socketId = pusher.connection.socket_id;
        console.log("got socket_id " + socketId);
        app.ports.socketIdAcquired.send(socketId);

        var en = 'private-' + channel_name;
        var channel = pusher.subscribe(en);

        // channel.bind("pusher:subscribe", function(data) {
        //   console.log("subscribe success: " + JSON.stringify(data));
        // });

        channel.bind("pusher:subscription_succeeded", function(data) {
          console.log("subscribe success: " + JSON.stringify(data));
        });

        channel.bind("contentUpdated", function (data) {
          // Method to be dispatched on trigger.
          // receive contentUpdated: {"socket_id":"133991.35181545","content":"123"}
          console.log("receive contentUpdated: " + JSON.stringify(data));
          app.ports.remoteContentUpdated.send(data);
        });

      });
    });


    if (app.ports && app.ports.askForConfirmation) {
      app.ports.askForConfirmation.subscribe(function () {
        app.ports.confirmations.send(window.confirm())
      });
    }
  </script>

### 为什么要写这样一个工具

当你远程登录一个系统，需要在机器之间复制文本时使用。你可以打开两个浏览器试试这个功能。 远程机器即使没有图形界面时可以应付一下。
[源代码](https://github.com/jianglibo/elm-mini-util)

<!-- ```bash
curl https://resp.me/channel/query?channel_name=968133317
```

```bash
curl 'https://resp.me/channel/post' -H 'Content-Type: application/json' \
  --data-raw '{"content":"dcc","socket_id":"133963.35875703","id":"483-804-225"}'
``` -->