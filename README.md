# Minimal WebRTC Messaging 
### Setting Up
We want to setup:
 - RTCPeerConnection
 - ICECandidate listener
 - DataChannel listener

```
connection = new RTCPeerConnection({"iceServers": [{"url":"stun:stun.1.google.com:19302"}]});

connection.onicecandidate = (e) => {
	if (e.candidate) {
		console.log('adding', JSON.stringify(e.candidate));
		connection.addIceCandidate(e.candidate);
	}
};

eDataChannel = undefined;

connection.ondatachannel = (e) => {
	if (e.channel) {
		console.log('found data channel');
		e.channel.onmessage = (e) => console.log(e.data);
	}
}

dataChannel = connection.createDataChannel('myDataChannel', { reliable: true });
```

### The Offer
 - Recording it locally
 - Recording it remotely

```
connection.createOffer(offer => {
	console.log(JSON.stringify(offer));
	connection.setLocalDescription(offer);
}, console.log);
```

### The Answer
 - Recording it locally
 - Recording it remotely

```
connection.setRemoteDescription(new RTCSessionDescription());
connection.addIceCandidate();
```
```
connection.createAnswer(answer => {
	console.log(JSON.stringify(answer));
	connection.setLocalDescription(answer);
}, console.log);
```
```
connection.setRemoteDescription(new RTCSessionDescription());
connection.addIceCandidate();
```
