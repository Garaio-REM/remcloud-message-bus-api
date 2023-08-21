# Header Properties
All message must contain following header properties:

Property | Type | Value | Example | Description
---|---|---|---|---
app_id | string | \<app_id> | headers: { app_id: 'YourService' }  | Identifies the sender of a message 

# Additional Header Properties
Some message or event specifications mention additional header properties. These are:

Property | Type | Value | Example | Description
---|---|---|---|---
recipient | string | <app_id> | headers: { recipient: 'rem_kunde_XYZ' } | Identifies the indented receiver of a message
local_authority | boolean | true or false | headers: { local_authority: true }  | A value of true indicates that an event can be forwarded to local authorities (Gemeinden)

## Echo Headers
Headers starting with 'echo-' are are copied verbatim into the accepted or rejected message.
