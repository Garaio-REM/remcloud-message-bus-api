# ContactData

Contact data of a person. This is a JSON hash structured as follows.

| Field                        | Type     | Content / Remarks                             |
| ---------------------------- | -------- | --------------------------------------------- |
| `privateEmails`              | `array`  | list of new private emails                    |
| `businessEmails`             | `array`  | list of new business emails                   |
| `otherEmails`                | `array`  | list of new other emails                      |
| `privatePhoneNumbers`        | `array`  | list of new private phone numbers             |
| `businessPhoneNumbers`       | `array`  | list of new business phone numbers            |
| `mobilePhoneNumbers`         | `array`  | list of new mobile phone numbers              |
| `otherPhoneNumbers`          | `array`  | list of new other phone numbers               |
| `privateFaxNumbers`          | `array`  | list of new private fax numbers               |
| `businessFaxNumbers`         | `array`  | list of new business fax numbers              |
| `otherFaxNumbers`            | `array`  | list of new other fax numbers                 |
| `privateUrls`                | `array`  | list of new private urls                      |
| `businessUrls`               | `array`  | list of new business urls                     |
| `otherUrls`                  | `array`  | list of new other urls                        |
| `contacts`                   | `array`  |                                               |
| &nbsp;&nbsp;`name`           | `string` | name of the contact                           |
| &nbsp;&nbsp;`contactAddress` | `string` | address / email / phone number of the contact |

## Merging strategy

When updating `ContactData`, fields are merged as follows:

* If you set a field, the existing entries in the given field will be replaced.
* If you set a field to `[]`, the existing entries in the given field are deleted.
* If you omit a field, the existing entries in the given field are left in place.

## Type Example

```
contactData": {
  "mobilePhoneNumbers": [
    { "value": "+41790001122" },
    { "value": "+41790005577" }
  ]
}
```

**NOTE**: Previously we accidentally accepted data not completely consistent to our described format and incorrectly and accepted the format:

```
contactData": {
  "mobilePhoneNumbers": [
    { 
      "value": "+41790001122", 
      "value": "+41790005577" 
    }
  ]
}
```

_This old format is **deprecated** and will be removed in a future release and now only accepts the first value in the hash._

_Strictly following the documented format allows us to implement new features associated with each value (in particular, email addresses)._

### Full Message Example 

to update contact data and document delivery preferences you can send a message with user's contact changes and document delivery preferences:

```
{
  "eventType": "Masterdata.Person.Update",
  "data": {
    "personReference": "100150",
    "modeOfDispatch": "email",
    "sendEmail": "john@example.com",
    "contactData": {
      "mobilePhoneNumbers": [
        { "value": "+41790001122" },
        { "value": "+41790005577" }
      ], 
      "privateEmails": [
        { "value": "john@example.com" },
        { "value": "jane@example.com" }
      ]
    }
  }
}
```

**NOTE**: `sendEmail` is the email address to which the document should be sent when `modeOfDispatch` is set to `email`.
