# SkyWay Peer Authentication Samples

This repository contains samples that show how to calculate the credentials for authenticating peers.

## Credential format

The credential passed to `new Peer()` is a JSON object in the following format.

```javascript
{
  authToken: <string>,
  ttl: <number>,
  timestamp: <number>
}
```

### ttl

The ttl is a value given in seconds between 600 (10 minutes) and 90000 (25 hours). After the ttl runs out, all connections to SkyWay servers are disconnected.

### timestamp

The timestamp is the current unix time (seconds).

*NOTE: A timestamp in the future will be rejected.*

### authToken

The authentication token for the peerId, calculated from the current peerId, the current timestamp and the ttl.
You can calculate it HMAC with H256 on the string `$timestamp:$ttl:$peerId`. You can find the secret key for your app on the developer's dashboard.
The final value MUST be in base64 string format.

## Using the samples

Look at the README in each individual language folder for information on how to run the server.

POST a request to the server at port `8080` to the `/authenticate` endpoint. The request must containing something authenticating the peer.
In the samples, we use a session token but it could also be a password.
The authentication with the session token is not implemented in the samples and always returns true.

### Example using JavaScript with JQuery

```javascript
$.post('http://localhost:8080/authenticate',
  {
    peerId: 'TestPeerID',
    sessionToken: '4CXS0f19nvMJBYK05o3toTWtZF5Lfd2t6Ikr2lID'
  }, function(credential) {
    var peer = new Peer('TestPeerID', {
      apikey: apikey,
      credential: credential
    });
    
    peer.on('open', function() {
      // ...
    });
  }).fail(function() {
    alert('Peer Authentication Failed');
  });
```




