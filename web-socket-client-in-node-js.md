# WebSocket Client Example

Install `ws` module:
```sh
npm init --yes
npm i -S ws
```

File `client.js`:
```js
'use strict';

const WebSocket = require('ws');
const ws = new WebSocket('ws://my.endpoint.uz/', {headers: {MyCustomHdr: 100200}});

ws.on('open', function open() {
    console.log('Connected');
    ws.send(data);
});

ws.on('close', function close() {
    console.log('Closed');
});

ws.on('error', function error(err) {
    console.log('Error=', err);
});

ws.on('message', function incoming(message) {
    console.log(message);
    ws.close();
});
```

Execute script:
```sh
node client.js
```

Sample output:
```
Connected
I'm a received data.
Closed
```
