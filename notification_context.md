# Notification Context

## Events

Type | GARAIO REM | REM | Description
---|---|---|---
[Notification.Message.Created](#notificationmessagecreated) | :white_check_mark: | :x: | A notification message has been created
[Notification.Case.Closed](#notificationcaseclosed) | :white_check_mark: | :x: | The case related to the externalReference has been closed

### Notification.Message.Created

This message represents a message from a messaging participant to a GARAIO REM instance. 

Set the **MESSAGE header** `recipient` to the target REM instance identifier (e.g. `grem_derham`). 
This controls which REM tenant queue receives the message.  This is **not** the notification recipient within REM, i.e. - full message with routing and properties:
```
{
  # routing and other Message properties - mentioned above
  exchange: "smart_invoice_publish",
  routing_key: "Notification.Message.Created",
  properties: [
    content_type: "application/json",
    timestamp: 1721215037,
    persistent: true,
    app_id: "smart_invoice",
    message_id: "smart_invoice-34567",
    headers: [app_id: "smart_invoice", recipient: "grem_derham"]
    #                                  ^^^^^^^^^^^^^^^^^^^^^^^^^^^
  ]
  # actual message data we process
  "eventType":"Notification.Message.Created",
  "data":{
    "externalReference":"12345",
    "masterdataReference":"67890",
    "mimetype":"text/plain",
    "message":"something bad happened"
  },
}
```

**NOTE**: messages are not allowed to have comments `#` in the JSON payload.

**Field** | **Type** | **Content / Remarks**
---|---|---
eventType | string | Notification.Message.Created
data | hash |
&nbsp;&nbsp;subject | string | Notification subject; optional if a masterdataReference is passed
&nbsp;&nbsp;backlinkUrl | string | optional url to navigate to the sending system; **must be a complete url that the local browser can resolve (including protocol), e.g. <https://www.google.com>**
&nbsp;&nbsp;categoryCode | string | optional for the category code - this value must match a 'code' within the 'notification_kategorie' of the CodeTabelle.  Blank or non-matching values will fall back to the default based on the sending system's name. (Ideally, in the future, these categories can be found via GraphQL, until they will be provided by Garaio REM personnel as needed).
&nbsp;&nbsp;externalReference | string | external, unique case identifier; **required**
&nbsp;&nbsp;masterdataReference | string | reference of a property / building / unit; **required** without **recipientUsername**
&nbsp;&nbsp;recipientResponsibility | string | the responsibility (role) on which to send the notification (1) (by default notifications are delivered to the `masterdataReference` **ASSISTANT**)
&nbsp;&nbsp;recipientUsername | string | optional recipient username (within GREM) to override the default recipient.  Blank or non-matching values will fallback to the default based on the given 'masterdataReference'. **required** without **masterdataReference**
&nbsp;&nbsp;mimetype | string | mimetype describing the message format (text/plain, text/markdown...); **required**
&nbsp;&nbsp;message | string | notification message; **required**
&nbsp;&nbsp;sender | string | optional sender info (email address, name...)

**NOTE**: A minimal `Notification.Message.Created` message requires one of the following: `masterdataReference` or `recipientUsername` or `recipientResponsibility` in order to determine the recipient.

**(1) Defined responsibilities**:

- ASSISTANT
- DATA_PROTECTION_OFFICER
- LETTING_MANAGER
- MARKETPLACE_EMAIL_MANAGER
- MARKETPLACE_MANAGER
- OWNER_PORTAL_CONTACT
- PROPERTY_MANAGER
- PROPERTY_ACCOUNTANT

#### Example

Minimal message **without `masterdataReference`**:

**requires** a `recipientResponsibility` or `recipientUsername` to determine the recipient.

```json
{"eventType":"Notification.Message.Created",
  "data":{
    "externalReference":"1234",
    "recipientUsername":"bookkeeper_wht",
    "subject":"Invoice 12345678.pdf",
    "mimetype":"text/plain",
    "message":"something went wrong"
  }
}
```


Minimal message **with `masterdataReference`**:

the default recipient is determined by the `masterdataReference` **ASSISTANT** (thus `recipientResponsibility` or `recipientUsername` are not required).

```json
{"eventType":"Notification.Message.Created",
  "data":{
    "externalReference":"1234",
    "masterdataReference":"4712.01.0001",
    "mimetype":"text/plain",
    "message":"something happened"
  }
}
```

Messages with `recipientResponsibility` (or `recipientUsername`) and `masterdataReference`:

Override the default recipient determined by the `masterdataReference`.

```json
{"eventType":"Notification.Message.Created",
  "data":{
    "externalReference":"1234",
    "masterdataReference":"4712.01.0001",
    "recipientResponsibility":"LETTING_MANAGER",
    "mimetype":"text/plain",
    "message":"something happened"
  }
}
```

**Typical Message**

```json
{"eventType":"Notification.Message.Created",
  "data":{
    "externalReference":"1234",
    "masterdataReference":"4712.01.0001",
    "sender":"max.muster@mailprovider.com",
    "mimetype":"text/plain",
    "message":"something happened",
    "backlinkUrl":"https://instance.external_system.ch/case/3"
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
    "externalReference":"1234"
  }
}
```
