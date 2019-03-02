
THE SETUP
We want to setup:
 - RTCPeerConnection
 - ICECandidate listener
 - DataChannel listener

THE OFFER
 - Recording it locally
 - Recording it remotely

THE ANSWER
 - Recording it locally
 - Recording it remotely

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


```
connection.createOffer(offer => {
	console.log(JSON.stringify(offer));
	connection.setLocalDescription(offer);
}, console.log);
```

```
connection.setRemoteDescription(new RTCSessionDescription( ));
connection.addIceCandidate();
```
```
connection.createAnswer(answer => {
	console.log(JSON.stringify(answer));
	connection.setLocalDescription(answer);
}, console.log);
```
```
connection.setRemoteDescription(new RTCSessionDescription( ));
connection.addIceCandidate();
```
