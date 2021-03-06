# peer-connector [![npm](https://img.shields.io/npm/v/peer-connector.svg)](https://www.npmjs.com/package/peer-connector)
A module to accept and request WebRTC connections by using WebSockets. <br>
Simple WebRTC video/voice/screen and data channels.

### Installing
```bash
$ npm install peer-connector
```

### Demo(sample test)
```bash
$ git clone https://github.com/goldenthumb/peer-connector.git
$ cd peer-connector
$ npm install
$ npm run dev

Now open this URL in your browser: http://localhost:3000/
```

## Usage
```js
// es6
import peerConnector from 'peer-connector';

// commonjs
var peerConnector = require('peer-connector');
```

```js
// es6

(async () => {
  try {
    const mediaType = { video: true, audio: true } // default mediaType
    // mediaType => video, audio, screen
    // screen can only be used by firefox and chrome(chrome requires chrome extensions).

    const config = { iceServers: [{ urls: 'stun:stun.l.google.com:19302' }] } // default config
    const servers = [
      {
        host,      // string required
        port,      // number required
        ssl,       // bool optional(default false)
        username,  // string optional
        password,  // string optional
      },
      ...
    ];

    const pc = await peerConnector({
      servers,     // required
      mediaType,   // optional
      config,      // optional
    });

    console.log(rtc.stream); // local stream;

    pc.on('connect', (peer) => {
      // peer is generated each time WebRTC is connected.
      console.log('peers info', pc.peers);

      // peer
      // {
      //   id => peer id
      //   localSdp => local sdp
      //   localStream => local stream
      //   remoteSdp => remote sdp
      //   remoteStream => remote stream
      //   on => event listener(open, close, message...)
      //   send => send data by data channel
      // }

      peer.on('stream', (stream) => {
        console.log(stream); // remote stream;
      });

      peer.on('open', () => {
        console.log('data channel open');
        peer.send('data channel connected');
      });

      peer.on('message', (data) => {
        console.log('message', data);
      });
    });
  } catch (e) {
    console.log(e);
  }
})();
```

## License
MIT

