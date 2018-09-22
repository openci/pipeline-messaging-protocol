---
description: >-
  A typical dependent node will check its cdpm yml file upon start up and then
  subscribe to each of its listed dependency nodes.
---

# Examples

The dependencies should respond back with a http 200 after the initial negotiation of the protocol.  When the dependency moves to a subscribed state, such as new release, it will publish the state as a message to all of its subscribed dependents.   The dependent, upon finishing its build will also respond with a promotion vote \(yes/no\) if its build is successful to the dependency.  


5.1 Registration  


Nodes  


Node Configuration Yaml File  


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


Dependents  


INITIAL state  


Subscribe HTTP POST to a dependency.    


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


BUILDING state  


A dependent state machine MUST change into a BUILDING state when receiving a new release message from its dependency.    


Dependency  


INITIAL state  


PUBLISHING state  


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


TESTING state  


DEPLOYING state  


DEPLOYED state  


\* needs dependency project name, url, event, state, new version, list of urls to artifacts

{

  "id": 42,

  "ref": "master",

  "created\_at": "2016-08-11T11:32:35.444Z",

  "project\_details": {

    “Id”: 32,

    "name": "Fog",

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


Major Dependents  


INITIAL state  


VOTE STABLE state  


VOTE UNSTABLE state  
  
  
  


![](https://lh6.googleusercontent.com/WbHU6mDh6tmQuVnS5_qgAphCWFde-YkJLpDH1lyoicFvu3IoSMWL28YCzPDOAh6QQNdHQRLalSINqz-GaYonkaVbZjQU6MeCB1uo5RzYufHHQM5q6N-8hi24sEHu0kuuA586QRlJ)

