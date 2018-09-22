---
description: >-
  The key words "MUST", "MUST NOT", "REQUIRED", "SHALL", "SHALL NOT", "SHOULD",
  "SHOULD NOT", "RECOMMENDED", "MAY", and "OPTIONAL" in this document are to be
  interpreted as described in [RFC2119].
---

# Terminology

2.1.  Definitions

CDPM Dependency:  A CDPM entity capable of listening for inbound connections from WebSocket clients and communicating using the WebSocket CDPM subprotocol as defined by this document.  Dependency nodes sit upstream from dependent nodes in that dependent nodes represent code bases that require the code represented by dependency nodes.

CDPM Dependent:  A CDPM entity capable of opening inbound and outbound connections to WebSocket servers and communicating using the WebSocket CDPM subprotocol as defined by this document.  Dependent nodes sit downstream from dependency nodes in that dependency nodes represent code bases that are required the code represented by dependent nodes.

CDPM State Machine:  A process that represents the state the CDPM entity is in.

