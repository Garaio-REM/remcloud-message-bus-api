# ContactData

## Field Structure (v1.27 - Current)

All contact data fields accept **objects** with the following structure:

| Field                  | Type    | Content / Remarks                                |
| ---------------------- | ------- | ------------------------------------------------ |
| `privateEmails`        | `array` | Array of email objects (private context)         |
| `businessEmails`       | `array` | Array of email objects (business context)        |
| `otherEmails`          | `array` | Array of email objects (other context)           |
| `privatePhoneNumbers`  | `array` | Array of phone number objects (private context)  |
| `businessPhoneNumbers` | `array` | Array of phone number objects (business context) |
| `mobilePhoneNumbers`   | `array` | Array of phone number objects (mobile context)   |
| `otherPhoneNumbers`    | `array` | Array of phone number objects (other context)    |
| `privateFaxNumbers`    | `array` | Array of fax number objects (private context)    |
| `businessFaxNumbers`   | `array` | Array of fax number objects (business context)   |
| `otherFaxNumbers`      | `array` | Array of fax number objects (other context)      |
| `privateUrls`          | `array` | Array of URL objects (private context)           |
| `businessUrls`         | `array` | Array of URL objects (business context)          |
| `otherUrls`            | `array` | Array of URL objects (other context)             |
| `contacts`             | `array` | Array of contact objects                         |

**Backward Compatibility** (v1.26 - Deprecated)

```json
{
  "privateEmails": ["email1@example.com", "email2@example.com"],
  "mobilePhoneNumbers": ["+41791234567", "+41798765432"]
}
```

**NOTE:** v1.26 format is supported through v1.27 (but cannot set an email to recieve documents):

### Object Structure for Communication Channels

Each communication channel (email, phone, fax, URL) is represented as an object:

| Field              | Type      | Required | Default | Description                                                      |
| ------------------ | --------- | -------- | ------- | ---------------------------------------------------------------- |
| `address`          | `string`  | Yes      | -       | The actual address (email, phone number, fax, URL)               |
| `comment`          | `string`  | No       | `null`  | A descriptive comment for this channel                           |
| `document_receipt` | `boolean` | No       | `false` | **For emails only**: If `true`, this email can receive documents |

### Object Structure for Contacts

| Field            | Type     | Required | Description                              |
| ---------------- | -------- | -------- | ---------------------------------------- |
| `name`           | `string` | Yes      | Name of the contact person               |
| `contactAddress` | `string` | Yes      | Contact information (email, phone, etc.) |

### Setting Document Delivery Email

To configure which email should receive documents, set `document_receipt: true` on the desired email:

```json
"contactData": {
  "businessEmails": [
    {
      "address": "documents@example.com",
      "comment": "Document delivery email",
      "document_receipt": true
    },
    {
      "address": "contact@example.com",
      "comment": "General contact"
    }
  ]
}
```

## Merging strategy

When updating `ContactData`, fields are merged as follows:

* If you set a field, the existing entries in the given field will be replaced.
* If you set a field to `[]`, the existing entries in the given field are deleted.
* If you omit a field, the existing entries in the given field are left in place.

## Type Example

```json
"contactData": {
  # new / replacement list of 'mobilePhoneNumbers'
  "mobilePhoneNumbers": [ 
    {"address":"+41798765432", "comment":"backup"}, 
    {"address":"+41791234567"} 
  ],
  "privateEmails": [], # deletes all existing 'privateEmails' data
  "businessEmails": [
    {"address":"primary@email.example.com", "comment":"primary", "document_receipt":true},
    {"address":"secondary@email.example.com", "comment":"secondary"},
    {"address":"terciary@email.example.com"}
  ],
  "contacts": [
    {"name": "John Doe", "contactAddress": "john.doe@example.com"},
    {"name": "Jane Doe", "contactAddress": "jane.doe@example.com"}
  ]
}
```

## Full Message Example / Document Delivery Configuration

**IMPORTANT**: Document delivery preferences are now controlled via the `document_receipt` field in `contactData`.
The attributes: `modeOfDispatch` and `sendEMail` are now ignored.  

```json
{
  "eventType": "Masterdata.Person.Update",
  "data": {
    "personReference": "100150",
    "contactData": {
      "mobilePhoneNumbers": [
        { "address": "+41798765432", "comment": "Backup phone" },
        { "address": "+41791234567" }
      ],
      "privateEmails": [
        { "address": "primary@example.com", "comment": "Primary email", "document_receipt": true },
        { "address": "secondary@example.com", "comment": "Secondary email" }
      ],
      "businessEmails": [
        { "address": "work@example.com", "comment": "Work email", "document_receipt": true }
      ],
      "otherEmails": [], # deletes all 'other' emails
      "contacts": [
        { "name": "John Doe", "contactAddress": "john.doe@example.com" },
        { "name": "Jane Doe", "contactAddress": "+41791234567" }
      ]
    }
  }
}
```

**NOTE:** when multiple emails have `"document_receipt": true` then the order of priority is:

1. the first **private email** with `"document_receipt": true` will be set to recieve documents per email
2. the first **business email** with `"document_receipt": true` will be set to recieve documents per email
3. the first **other email** with `"document_receipt": true` will be set to recieve documents per email

If there are no emails with `"document_receipt": true` then no documents can be sent via email (unless `"document_receipt": true` is already set in an email category the doesn't change)
