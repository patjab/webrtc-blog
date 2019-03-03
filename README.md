# Minimal WebRTC Messaging 
This markdown document lists the three parts needed for creating an RTCDataChannel: setting up the listeners, creating and receiving the offer, creating and receiving the answer.

### Setting Up
 - RTCPeerConnection
 - ICECandidate listener
 - DataChannel listener
 
 (without TURN: stun:stun.1.google.com:19302)
 
 (with TURN: http://numb.viagenie.ca:3478)

```
connection = new RTCPeerConnection({
	"iceServers": [
		{
			url: "stun:stun.1.google.com:19302"
		},
		{
			url: "turn:numb.viagenie.ca:3478",
			username: "wireless.patrick@gmail.com",
			credential: "zA9Tu3feUmb"
		}
	]
});

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

```
connection.setRemoteDescription(new RTCSessionDescription());
connection.addIceCandidate();
```

### The Answer
 - Recording it locally
 - Recording it remotely
 
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
