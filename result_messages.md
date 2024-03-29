# Result messages

For interactions with GARAIO REM that follow a request/response pattern, the following result message types are commonly sent.

## Response mapping using correlation_id

These messages set the AMQP property `correlation_id` to contain the `message_id` of the request message.
This means you can tell to what request message an `Accepted` or `Rejected` belongs by remembering the `message_id` of sent requests and comparing them to the `correlation_id` of received responses.

## Accepted message

Named `*.Accepted`, this is sent by GARAIO REM to signal that a message was successfully processed.

Common fields are listed below. The various accepted messages might add additional fields to `data`.

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | Name of the message, e.g. "Letting.Tenancy.Accepted"
`data` | `hash` |

## Rejected message

Named `*.Rejected`, this is sent by GARAIO REM to signal that a message was rejected and not processed.
GARAIO REM validation errors are mapped into the reasons array

Common fields are listed below. The various accepted messages might add additional fields to `data`.

Shared fields:

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | Name of the message, e.g. "Letting.Tenancy.Rejected"
`data` | `hash` |
&nbsp;&nbsp;`reasons` | `array` |
&nbsp;&nbsp;&nbsp;&nbsp;`attribute` | `string` | name of the message attribute, eg. "masterdataReference"; might be null if the reason does not map to a specific attribute
&nbsp;&nbsp;&nbsp;&nbsp;`reason` | `string` | reason, eg. "ist nicht bekannt"
