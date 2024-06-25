# Notification Context

## Events

Type | GARAIO REM | REM | Description
---|---|---|---
[Notification.Message.Created](#notificationmessagecreated) | :white_check_mark: | :x: | A notification message has been created
[Notification.Case.Closed](#notificationcaseclosed) | :white_check_mark: | :x: | The case related to the externalReference has been closed

### Notification.Message.Created

This message represents a message from a messaging participant to a GARAIO REM instance. Set the recipient property in the headers, eg "grem_derham"

Field | Type | Content / Remarks
---|---|---
eventType | string | Notification.Message.Created
data | hash |
&nbsp;&nbsp;externalReference | string | external, unique case identifier; **required**
&nbsp;&nbsp;masterdataReference | string | reference of a property / building / unit; **required**
&nbsp;&nbsp;categoryCode | string | optional for the category code - this value must match a 'code' within the 'notification_kategorie' of the CodeTabelle.  Blank or non-matching values will fall back to the default based on the sending system's name. (Ideally, in the future, these categories can be found via GraphQL, until they will be provided by Garaio REM personnel as needed).
&nbsp;&nbsp;recipientUsername | string | optional recipient username (within GREM) to override the default recipient.  Blank or non-matching values will fallback to the default based on the given 'masterdataReference'.
&nbsp;&nbsp;sender | string | optional sender info (email address, name...)
&nbsp;&nbsp;mimetype | string | mimetype describing the message format (text/plain, text/markdown...); **required**
&nbsp;&nbsp;message | string | notification message; **required**
&nbsp;&nbsp;backlinkUrl | string | REQUIRED url to navigate to the sending system; **must be a complete url that the local browser can resolve (including protocol), e.g. <https://www.google.com>**

#### Example

```json
{"eventType":"Notification.Message.Created",
  "data":{
    "externalReference":"1234",
    "masterdataReference":"4712.01.0001",
    "sender":"max.muster@mailprovider.com",
    "mimetype":"text/plain",
    "message":"something happened",
    "backlinkUrl":"https://instance.external_system.ch/case/3",
  }
}
```

### Notification.Case.Closed

This message represents a message from a messaging participant to a GARAIO REM instance notifying that the case related to the externalReference has been closed. Set the recipient property in the headers, eg "grem_derham".

Field | Type | Content / Remarks
---|---|---
eventType | string | Notification.Case.Closed
data | hash |
&nbsp;&nbsp;externalReference | string | external, unique case identifier; **required**

#### Example

```json
{"eventType":"Notification.Case.Closed",
  "data":{
    "externalReference":"1234",
  }
}
```
