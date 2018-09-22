---
description: >-
  The term WebSocket subprotocol refers to an application-level protocol layered
  on top of a WebSocket connection.
---

# The WebSocket CDPM Subprotocol

This document specifies the WebSocket CDPM subprotocol for carrying CDPM requests and responses through a WebSocket connection.

4.1    Handshake  


The CDPM WebSocket Client and CDPM WebSocket Server negotiate usage of the WebSocket CDPM subprotocol during the WebSocket handshake procedure as defined in Section 1.3 of \[RFC6455\].  The client MUST include the value "cdpm" in the Sec-WebSocket-Protocol header in its handshake request. The 101 reply from the server MUST contain "cdpm" in its corresponding Sec-WebSocket-Protocol header.  
  
The WebSocket client initiates a WebSocket connection when attempting to send a CDPM request \(unless there is an already established WebSocket connection for sending the CDPM request\).  In case there is no HTTP 101 response during the WebSocket handshake, it is considered a transaction error as per \[RFC3261\], Section 8.1.3.1., "Transaction Layer Errors".  
  
Below is an example of a WebSocket handshake in which the client requests the WebSocket CDPM subprotocol support from the server:  
  
     GET / HTTP/1.1  
     Host: cdpm-ws.example.com  
     Upgrade: websocket  
     Connection: Upgrade  
     Sec-WebSocket-Key: dGhlIHNhbXBsZSBub25jZQ==  
     Origin: http://www.example.com  
     Sec-WebSocket-Protocol: cdpm  
     Sec-WebSocket-Version: 13  
  
   The handshake response from the server accepting the WebSocket CDPM  
   subprotocol would look as follows:  
  
     HTTP/1.1 101 Switching Protocols  
     Upgrade: websocket  
     Connection: Upgrade  
     Sec-WebSocket-Accept: s3pPLMBiTxaQ9kYGzzhZRbK+xOo=  
     Sec-WebSocket-Protocol: cdpm  
  


Once the negotiation has been completed, the WebSocket connection is established and can be used for the transport of CDPM requests and responses. Messages other than CDPM requests and responses MUST NOT be transmitted over this connection.  
  
4.2    CDPM Encoding  
  


WebSocket messages can be transported in either UTF-8 text frames or binary frames. CDPM only allows text bodies in CDPM requests and responses. Therefore, CDPM WebSocket Clients and CDPM WebSocket Servers MUST only accept text frames.  
  


Given the nature of JavaScript and the WebSocket API, it is RECOMMENDED to use UTF-8 encoding \(or ASCII, which is a subset of UTF-8\) for CDPM messages carried over a WebSocket connection.  
  


4.3      CDPM Protocol  


Nodes  


A pipeline messaging state machine MUST have a node.  There MAY be zero, one, or many state machines running on a node.  A state machine MUST be designated either a dependency or a dependent state machine.  A state machine MUST be assigned a link to a yml file, in source control, with the semantically versioned dependency or dependent source code.  A state machine MUST include a rsa256 public key in its yml configuration file \(off band\). A node must designate its location in its yml configuration file.  A state machine’s events MAY be triggered by anything the CDPM implementation deems as a valid event from a corresponding CI/CD system.  


Node Configuration Yaml File  


A node’s yml file MUST be in source control with the semantically versioned dependency or dependent source code.  


GET /rails/rails/blob/master/cdpm.yml HTTP/1.1  
Host: github.com  


Pipeline-node:

  public-key: MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQCqGKukO1De7zhZj6+H0qtjTkVxwTCpvKe4eCZ0FPqri0cb2JZfXJDgYSF6vUpwmJG8wVQZKjeGcjDOL5UlsuusFncCzWBQ7RKNUSesmQRMSGkVb13j+skZ6UtW+5u09lHNsj6tQ51s1SPrCBkedbNf0Tp0GbMJDyR4e9T04ZZwIDAQAB

  url: [https://rails.ci](https://rails.ci)  


Dependencies:

  Nokogiri-master:

    url: https://github.com/sparklemotion/nokogiri/blob/master/cdpm.yml

  Nokogiri-stable:

    url: https://github.com/sparklemotion/nokogiri/blob/v1.8.1/cdpm.yml

Dependents:

  hulu-master:

    url: http://mysourcecontrol.com/hulu/blob/master/cdpm.yml  


Messages  


| Message Name | Message Description |
| :--- | :--- |
| subscribe | HTTP POST to a dependency |
| subscribed | HTTP Response to a dependent |
| new release | Build, test, and release finished message to dependent or dependency |
| vote unstable | Build, test, or release failed message to dependency |
| vote stable | Release successful message to dependency |

Messaging Security  


Every message between nodes MUST include a signature signed by the sending node’s rsa 256 public key.  Every node that receives a message MUST verify the message using the off band public key originally found sender’s yml file in source control.  A dependency MAY process a subscribe message from a dependant node.  A dependency MUST verify a subscribe message using the public key found using the link in the subscribe message to the dependant’s sourced controlled yml file.   Every dependency node MUST reject any message, other than a subscription message, from unsubscribed nodes. Every dependent node MUST reject any message from nodes not subscribed to.  
  


<table>
  <thead>
    <tr>
      <th style="text-align:left">State Machine</th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
      <th style="text-align:left"></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td style="text-align:left">
        <p>Dependent</p>
        <p>example</p>
      </td>
      <td style="text-align:left">Hulu</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Major</p>
        <p>Dependent</p>
        <p>example</p>
      </td>
      <td style="text-align:left">Rails</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">
        <p>Dependency</p>
        <p>example</p>
      </td>
      <td style="text-align:left">Nokogiri</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">Event</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Node</td>
      <td style="text-align:left">Subscribe dependency</td>
      <td style="text-align:left">
        <p>Dependency</p>
        <p>Check-in</p>
      </td>
      <td style="text-align:left">Built</td>
      <td style="text-align:left">Tested</td>
      <td style="text-align:left">
        <p>New</p>
        <p>release</p>
      </td>
      <td style="text-align:left">
        <p>Tests</p>
        <p>Failed</p>
      </td>
      <td style="text-align:left">
        <p>Tests</p>
        <p>Success</p>
      </td>
    </tr>
    <tr>
      <td style="text-align:left">Dependent</td>
      <td style="text-align:left">subscribed</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">building</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
    </tr>
    <tr>
      <td style="text-align:left">Stability</td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left"></td>
      <td style="text-align:left">vote unstable</td>
      <td style="text-align:left">vote stable</td>
    </tr>
    <tr>
      <td style="text-align:left">Project dependency</td>
      <td style="text-align:left">publishing</td>
      <td style="text-align:left">building</td>
      <td style="text-align:left">testing</td>
      <td style="text-align:left">
        <p>deploying</p>
        <p>production</p>
      </td>
      <td style="text-align:left">deployed</td>
      <td style="text-align:left">demoted</td>
      <td style="text-align:left">promoted
        <br />
      </td>
    </tr>
  </tbody>
</table>Dependents

A dependent node MAY subscribe to a dependency node.  Subscribable dependency nodes MUST be included as links in a dependent yml configuration file, in source control, versioned with the dependent’s source code.  A link to a subscribable dependency MUST be a link to a yml file, in source control, with the semantically versioned dependency source code.  


INITIAL state  


A dependent state machine MUST be in the initial state when initialized.  A dependent state machine in the INITIAL state MAY send a subscribe HTTP POST to a dependency.    


Dependent Subscription POST  


POST /subscribe HTTP/1.1  
Host: nokogirl.ci

{  
    “message\_type”:”subscribe”,

    "caller\_name": "rails",

    "url": "http://www.github.com/rails/rails/blob/master/cdpm.yml",  
    "subscriptions": \[  
        {  
            “ref\_type”: “stable”,

            “ref”: “v\_3.2”,     
        }  
    \],  
    "destination": {  
        "url": "https://github.com/sparklemotion/nokogiri/blob/master/cdpm.yml  
    }  
}  


SUBSCRIBED state  


A dependent state machine MUST change into the SUBSCRIBED state after receiving a subscribed message from its dependency AND it is in the initial state.    


Dependent Subscribed POST response  


{

    “message\_type”:”subscribed”,

    "response": \[

        {"subscription\_id": "e6a9bb54-da25-102b-9a03-2db401e887ec"}

    \],

    "status\_code": "200",

    "status\_msg": "OK",

    "response\_time\_stamp": "2012-06-20T08:36:24"

}  


A dependent node in the SUBSCRIBED state MUST negotiate a websocket connection with the dependency node.  
  


BUILDING state  


A dependent state machine MUST change into a BUILDING state when receiving a new release message from its dependency.    
  


Dependency  


A dependency’s source code MUST include a yml configuration file with the url and port address of the pipeline messaging node.  A dependencies yml file MUST designate a reference. A reference MUST be either a branch or a tag. A reference that is a tag MUST be a semantic version.  


INITIAL state  


A dependency state machine MUST be in the INITIAL state when initialized.    


PUBLISHING state  


A dependency MAY send a subscribed message to a dependent when receiving a subscribe message.  A dependency MUST change its stage from INITIAL to PUBLISHING when it sends a subscribed message to the dependent  A dependency MAY send a not subscribed message to the dependent when receiving a subscribe message.  


Dependent New Release Subscription POST response  


{

    “message\_type”:”subscribed”,

    "response": \[

        {"subscription\_id": "e6a9bb54-da25-102b-9a03-2db401e887ec"}

    \],

    "status\_code": "200",

    "status\_msg": "OK",

    "response\_time\_stamp": "2012-06-20T08:36:24"

}

[Source](https://docs.bmc.com/docs/display/public/btsim96/Using+the+Publish-Subscribe+REST+approach+to+receive+events)

CHECK-IN state

A dependency MUST change its state to BUILDING when a code check-in event takes place.

TESTING state

A dependency MUST change its state to TESTING when a built event takes place.

DEPLOYING state

A dependency MAY change its state to DEPLOYING when a tested event takes place.

DEPLOYED state

A dependency MUST change its state to DEPLOYED when a new release event takes place.  When a dependency has changed into a DEPLOYED state, it must make its artifacts available and then send a NEW RELEASE message to all of its

dependents.  


\* needs dependency project name, url, event, state, new version, list of urls to artifacts

{

  "id": 42,

  "ref": "master",

  "created\_at": "2016-08-11T11:32:35.444Z",

  "project\_details": {

    “Id”: 32,

    "name": "nokogiri",

    "repo\_url": "[https://rubygems.org](https://rubygems.org)"

    “maintainers” : \[{

       “name”: “joe blow”

       “contact”: “joe@email.com”

     }\],

  “dependents”: \[“rails”, “sinatra”\]

  },

  "deployable": {

    "id": 664,

    "status": "success",

    "ref": "master",

    "created\_at": "2016-08-11T11:32:24.456Z",

    "commit": {

      "id": "a91957a858320c0e17f3a0eca7cfacbff50ea29a",

      "short\_id": "a91957a8",

      "title": "2.3 update",

      "author\_name": "joe blow",

      "author\_email": "joe@email.com",

      "created\_at": "2016-08-11T13:28:26.000+02:00",

      "message": "update"

    }

  }

}  


DEMOTED state  


A dependency MAY change its state to DEMOTED when one or many vote unstable messages are received.

{

    "response": \[

        {"subscription\_id": "e6a9bb54-da25-102b-9a03-2db401e887ec"}

    \],

    "status\_code": "200",

    "status\_msg": "OK",

    "response\_time\_stamp": "2012-06-20T08:36:24"

}

PROMOTED state

A dependency MAY change its state to PROMOTED when one or many vote stable messages are received.

Major Dependents

A dependent MAY be designated as a major dependent by its dependency.   A major dependent MUST have an additional ‘stability’ state machine separate from the dependent state machine. A major dependent MUST message its dependency when its stability state changes to VOTE STABLE or VOTE UNSTABLE

INITIAL state

A major dependent MUST be in the INITIAL state when initialized

VOTE STABLE state

A major dependent MUST change its state to VOTE STABLE when tests success event occurs with a dependency reference.  A dependent MUST send a vote stable message to the dependency when entering into a vote stable state.

VOTE UNSTABLE state

A major dependent MUST change its state from VOTE STABLE to VOTE UNSTABLE when a tests failure event occurs and the dependent new release event was triggered by a message from the dependency.  A major dependent MUST NOT change its state from INITIAL to VOTE UNSTABLE. A dependent MUST send a vote unstable message to the dependency when entering into a vote unstable state.

