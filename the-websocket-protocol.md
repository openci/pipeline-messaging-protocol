---
description: >-
  The WebSocket protocol [RFC6455] is a transport layer on top of TCP
  (optionally secured with TLS [RFC5246]) in which both client and server
  exchange message units in both directions.
---

# The WebSocket Protocol

The protocol defines connection handshake, WebSocket subprotocol and extensions negotiation, a frame format for sending application and control data, a masking mechanism, and status codes for indicating disconnection causes.

The WebSocket connection handshake is based on HTTP \[[RFC2616](https://tools.ietf.org/html/rfc2616)\] and utilizes the HTTP GET method with an "Upgrade" request.  This is sent by the client and then answered by the server \(if the negotiation succeeded\) with an HTTP 101 status code.  Once the handshake is completed, the connection upgrades from HTTP to the WebSocket protocol. This handshake procedure is designed to reuse the existing HTTP infrastructure.  During the connection handshake, the client and server agree on the application protocol to use on top of the WebSocket transport. Such an application protocol \(also known as a "WebSocket subprotocol"\) defines the format and semantics of the messages exchanged by the endpoints.  This could be a custom protocol or a standardized one \(as defined by the WebSocket SIP subprotocol in this document\). Once the HTTP 101 response is processed, both the client and server reuse the underlying TCP connection for sending WebSocket messages and control frames to each other.  Unlike plain HTTP, this connection is persistent and can be used for multiple message exchanges.

WebSocket defines message units to be used by applications for the exchange of data, so it provides a message-boundary-preserving transport layer.  These message units can contain either UTF-8 text or binary data and can be split into multiple WebSocket text/binary transport frames as needed by the WebSocket stack.  
  
The WebSocket API \[[WS-API](https://tools.ietf.org/html/rfc7118#ref-WS-API)\] for web browsers only defines callbacks to be invoked upon receipt of an entire message unit, regardless of whether it was received in a single WebSocket frame or split across multiple frames.

