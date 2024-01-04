# Sedex Context
Sedex Messages are not meant to be implemented by external services.

## Events
Type | Status | Description
---|---|---
[Sedex.Messaging.MessageReceived](#sedexmessagingmessagereceived) | :white_check_mark: | A message has been received from Sedex

## Commands
Request/Reply|Type | Status | Description |
---|---|--- |---
Request | [Sedex.Messaging.SendMessage](#sedexmessagingsendmessage) | :white_check_mark: | Requests a message being sent over Sedex
Reply | [Sedex.Messaging.MessageSent](#sedexmessagingmessagesent) | :white_check_mark: | The Sedex-Receipt confirming a message
Request | [Sedex.Messaging.CheckSedex](#sedexmessagingchecksedex) | :white_check_mark: | Requests checking a recipient can receive a message
Reply | [Sedex.Messaging.SedexChecked](#sedexmessagingsedexchecked) | :white_check_mark: | Reply to the requested check

## Details
### Sedex.Messaging.MessageReceived
Field | Type | Content / Remarks
---|---|---
eventType | string | Sedex.Messaging.Received
data | hash |
&nbsp;&nbsp;envelope | hash | message envelope
&nbsp;&nbsp;&nbsp;&nbsp;message_id | string | eCH-0090 message id
&nbsp;&nbsp;&nbsp;&nbsp;message_type | string | eCH-0090 message type
&nbsp;&nbsp;&nbsp;&nbsp;message_class | string | eCH-0090 message class
&nbsp;&nbsp;&nbsp;&nbsp;sender_id | string | eCH-0090 sender id
&nbsp;&nbsp;&nbsp;&nbsp;recipient_id | string | eCH-0090 recipient id
&nbsp;&nbsp;data | hash | message data
&nbsp;&nbsp;&nbsp;&nbsp;content| string | content of the received data file

#### Additional Header Properties
#### Example
```json
{
  "event_type": "Sedex.Messaging.MessageReceived",
  "data": {
    "envelope": {
      "message_id": "1999414",
      "message_type": "1170",
      "message_class": "7",
      "sender_id": "T4-4444123-5",
      "recipient_id": "T4-1331302-4"
    },
    "content": "Get me a coffee please"
  }
}
```

### Sedex.Messaging.SendMessage
Field | Type | Content / Remarks
---|---|---
eventType | string | Sedex.Messaging.SendMessage
data | hash ||
&nbsp;&nbsp;envelope | hash | message envelope
&nbsp;&nbsp;&nbsp;&nbsp;message_type | string | eCH-0090 message type
&nbsp;&nbsp;&nbsp;&nbsp;message_class | string | eCH-0090 message class
&nbsp;&nbsp;&nbsp;&nbsp;sender_id | string | eCH-0090 sender id
&nbsp;&nbsp;&nbsp;&nbsp;recipient_id | string | eCH-0090 recipient id
&nbsp;&nbsp;data | hash | message data
&nbsp;&nbsp;&nbsp;&nbsp;format | string | file extension for the transmitted data file
&nbsp;&nbsp;&nbsp;&nbsp;content| string | content for the transmitted data file

The message_id, which is part of every message, is used here as id for the message sent to Sedex. This id will later
be returned by Sedex in the receipt (see [Sedex.Messaging.MessageSent](#sedexmessagingmessagesent)). The id used must
therefore be globally unique.
#### Example
```json
{
    "eventType": "Sedex.Messaging.SendMessage",
    "data": {
        "envelope": {
            "messageType": "782",
            "messageClass": "4",
            "senderId": "T4-55123-5",
            "recipientId": "T4-998812-1"
        },
        "data": {
            "format": "xml",
            "content": "<xml>Hello<\\xml>"
        }
    }
}
```

### Sedex.Messaging.MessageSent
Field | Type | Content / Remarks
---|---|---
eventType | string | Sedex.Messaging.MessageSent
data | hash ||
&nbsp;&nbsp;eventData | string | ISO 8601 encoded date when message was received by recipient
&nbsp;&nbsp;statusCode | string | eCH-0090 status code
&nbsp;&nbsp;statusInfo | string | eCH-0090 status info
&nbsp;&nbsp;messageId | string | message_id with which the message was transmitted
&nbsp;&nbsp;messageType | string | eCH-0090 message type of the message transmitted
&nbsp;&nbsp;messageClass | string | eCH-0090 message class of the message transmitted
&nbsp;&nbsp;senderId | string |  eCH-0090 sender id of the message transmitted
&nbsp;&nbsp;recipientId| string | eCH-0090 recipient id of the message transmitted
#### Additional Message Properties
Property | Remarks
---|---
correlationId | messageId of the correlating request
#### Example
```json
{
  "eventType": "Sedex.Messaging.MessageSent",
  "data": {
    "eventDate": "2020-10-09T06:25:37+01:00",
    "statusCode": "300",
    "statusInfo": "ssl invalid",
    "messageId": "88013322",
    "messageType": "782",
    "messageClass": "4",
    "senderId": "T4-55123-5",
    "recipientId": "T4-998812-1"
}
```
### Sedex.Messaging.CheckSedex
Field | Type | Content / Remarks
---|---|---
eventType | string | Sedex.Messaging.CheckSedex
data | hash ||
&nbsp;&nbsp;messageTyp | string| eCH-0090 message type to check
&nbsp;&nbsp;messageClass | string | eCH-0090 message class to check
&nbsp;&nbsp;recipientSedexId | string | eCH-0090 recipient sedex id to check
#### Example
```json
{
    "eventType": "Sedex.Messaging.CheckSedex",
    "data": {
        "messageType": "780",
        "messageClass": "4",
        "recipientSedexId": "T1-1119-2"
    }
}
```

### Sedex.Messaging.SedexChecked
Field | Type | Content / Remarks
---|---|---
eventType | string | Sedex.Messaging.MessageSent
data | hash ||
&nbsp;&nbsp;errorCode | string | result of the check (see [Sedex Betriebs-/ Integrationshandbuch](https://www.bfs.admin.ch/bfsstatic/dam/assets/315862/master))
&nbsp;&nbsp;errorMessage | string | additional info when check failed
&nbsp;&nbsp;messageTyp | string| eCH-0090 message type checked
&nbsp;&nbsp;messageClass | string | eCH-0090 message class checked
&nbsp;&nbsp;recipientSedexId | string | eCH-0090 recipient sedex id checked
#### Additional Message Properties
Property | Remarks
---|---
correlationId | messageId of the correlating request
#### Example
```json
{
    "eventType": "Sedex.Messaging.SedexChecked",
    "data": {
        "errorCode": "301",
        "errorMessage": "Unknown recipient id T1-6748-1",
        "messageType": "780",
        "messageClass": "4",
        "recipientSedexId": "T1-1119-2"
    }
}
```
