# Minimal WebRTC Messaging 
This markdown document lists the three parts needed for creating an RTCDataChannel: setting up the listeners, creating and receiving the offer, creating and receiving the answer.

### Setting Up
 - RTCPeerConnection
 - ICECandidate listener
 - DataChannel listener
 
```
const connection = new RTCPeerConnection({ iceServers: [url: 'stun:stun.1.google.com:19302'] });

connection.onicecandidate = (e) => {
	if (e.candidate) {
		console.log('CANDIDATE: ', JSON.stringify(e.candidate));
		connection.addIceCandidate(e.candidate);
	}
};

let eDataChannel = undefined;

connection.ondatachannel = (e) => {
	if (e.channel) {
		console.log('Data Channel now open');
		e.channel.onmessage = (e) => console.log(e.data);
	}
}

const dataChannel = connection.createDataChannel('myDataChannel', { reliable: true });
```

### The Offer
 - Recording it locally
 - Recording it remotely

```
connection.createOffer(offer => {
	console.log('OFFER: ', JSON.stringify(offer));
	connection.setLocalDescription(offer);
}, console.log);
```

```
connection.setRemoteDescription(new RTCSessionDescription());
connection.addIceCandidate();
```

### The Answer
 - Recording it locally
 - Recording it remotely
 
```
connection.createAnswer(answer => {
	console.log('ANSWER: ', JSON.stringify(answer));
	connection.setLocalDescription(answer);
}, console.log);
```
```
connection.setRemoteDescription(new RTCSessionDescription());
connection.addIceCandidate();
```
