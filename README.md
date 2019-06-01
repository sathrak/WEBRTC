# Real time communication with WebRTC

## Introduction
  WebRTC is an open source project to enable realtime communication of audio, video and data in Web and native apps.

## What is signaling?
  WebRTC uses RTCPeerConnection to communicate streaming data between browsers, but also needs a mechanism to coordinate communication and to send control messages, a process known as signaling. Signaling methods and protocols are not specified by WebRTC.
    
 ## What are STUN and TURN?
  WebRTC is designed to work peer-to-peer, so users can connect by the most direct route possible. However, WebRTC is built to cope with real-world networking: client applications need to traverse NAT gateways and firewalls, and peer to peer networking needs fallbacks in case direct connection fails. As part of this process, the WebRTC APIs use STUN servers to get the IP address of your computer, and TURN servers to function as relay servers in case peer-to-peer communication fails.
  
## WebRTC has several JavaScript APIs -



RTCPeerConnection:
==================

	The RTCPeerConnection interface represents a WebRTC connection between the local computer and a remote peer. It provides methods to connect to a remote peer, maintain and monitor the connection, and close the connection once it's no longer needed.

	var RTCConn = new RTCPeerConnection(servers);

Media Permission :
==================
	The MediaDevices.getUserMedia() method prompts the user for permission to use a media input which produces a MediaStream with tracks containing the requested types of media.

	Examle: 

		navigator.mediaDevices.getUserMedia(constraints)
		.then(function(stream) {
		  /* use the stream */
		})
		.catch(function(err) {
		  	/* handle the error */			
			callend(6);
			var errobj = {code:err.code,name:err.name,message:err.message};
			WebRtcWebResponse.WebrtcResponse(JSON.stringify(errobj), 'Error', 6);
		});

AddTrack :
==========
	The RTCPeerConnection method addTrack() adds a new media track to the set of tracks which will be transmitted to the other peer.
	Example : 
		stream.getTracks().forEach(track => pc.addTrack(track, stream));

CreateOffer :
=============
	The createOffer() method of the RTCPeerConnection interface initiates the creation of an SDP offer for the purpose of starting a new WebRTC connection to a remote peer.

	Example:

		RTCConn.createOffer(function (offer) {
			 /* use the offer */
		},function (err) {
			callend(8);
			var errobj = {code:err.code,name:err.name,message:err.message};
			WebRtcWebResponse.WebrtcResponse(JSON.stringify(errobj), 'Error', 8);
		});

SetLocalDescription:
====================
	The RTCPeerConnection.setLocalDescription() method changes the local description associated with the connection. This description specifies the properties of the local end of the connection, including the media format. The method takes a single parameter—the session description—and it returns a Promise which is fulfilled once the description has been changed, asynchronously.

	Example:

		RTCConn.setLocalDescription(offer,function() { 
			/* use the offer */
		},function(err) {
			// An error occurred, so handle the failure to connect
			callend(9);
			var errobj = {code:err.code,name:err.name,message:err.message};
			WebRtcWebResponse.WebrtcResponse(JSON.stringify(errobj), 'Error', 9); 
		});

SetRemoteDescription :
======================

The RTCPeerConnection.setRemoteDescription() method changes the remote description associated with the connection. This description specifies the properties of the remote end of the connection, including the media format. The method takes a single parameter—the session description—and it returns a Promise which is fulfilled once the description has been changed, asynchronously.


Example : 
var desc = new RTCSessionDescription(CallerOffer);
RTCConn.setRemoteDescription(desc, function() {
	/* use the CallerOffer */
},function(err) {
	// An error occurred, so handle the failure to connect						
	callend(15);	
	var errobj = {code:err.code,name:err.name,message:err.message};
	WebRtcWebResponse.WebrtcResponse(JSON.stringify(errobj), 'Error', 15);
});


CreateAnswer:
=============
	The createAnswer() method on the RTCPeerConnection interface creates an SDP answer to an offer received from a remote peer during the offer/answer negotiation of a WebRTC connection. The answer contains information about any media already attached to the session, codecs and options supported by the browser, and any ICE candidates already gathered. The answer is delivered to the returned Promise, and should then be sent to the source of the offer to continue the negotiation process.

	Example : 
		RTCConn.createAnswer().then(function(answer) {
			return RTCConn.setLocalDescription(answer);
		}).then(function() {
			// Send the answer to the remote peer using the signaling server
			WebRtcWebResponse.WebrtcResponse(JSON.stringify(RTCConn.localDescription), 'answer', 2);
		}).catch(function(err) {
			// An error occurred, so handle the failure to connect
			callend(14);
			var errobj = {code:err.code,name:err.name,message:err.message};
			WebRtcWebResponse.WebrtcResponse(JSON.stringify(errobj), 'Error', 14);
		});

onicecandidate:
===============
	The RTCPeerConnection.onicecandidate property is an EventHandler which specifies a function to be called when the icecandidate event occurs on an RTCPeerConnection instance. This happens whenever the local ICE agent needs to deliver a message to the other peer through the signaling server. This lets the ICE agent perform negotiation with the remote peer without the browser itself needing to know any specifics about the technology being used for signaling; simply implement this method to use whatever messaging technology you choose to send the ICE candidate to the remote peer.

	RTCConn.onicecandidate = function(event) {
		if (event.candidate) {
			// Send the candidate to the remote peer
			WebRtcWebResponse.WebrtcResponse(JSON.stringify(event.candidate),'iceCallInit',3);
		}else{
			// All ICE candidates have been sent
		}
	}
