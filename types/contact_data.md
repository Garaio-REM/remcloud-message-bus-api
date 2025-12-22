# ContactData

Contact data of a person. This is a JSON hash structured as follows.

| Field                        | Type     | Content / Remarks                                                            |
| ---------------------------- | -------- | ---------------------------------------------------------------------------- |
| `privateEmails`              | `array`  | list of new private emails: `["email@example1.com","email@example2.com"]`    |
| `businessEmails`             | `array`  | list of new business emails: `["email@example1.com","email@example2.com"]`   |
| `otherEmails`                | `array`  | list of new other emails: `["email@example1.com","email@example2.com"]`      | 
| `privatePhoneNumbers`        | `array`  | list of new private phone numbers: ["+41793456789","+41798765432"]           |
| `businessPhoneNumbers`       | `array`  | list of new business phone numbers: `["+41312345678","41318765432"]`         |
| `mobilePhoneNumbers`         | `array`  | list of new mobile phone numbers: `["+41793456789","+41798765432"]`          |
| `otherPhoneNumbers`          | `array`  | list of new other phone numbers: `["+41312345678","41318765432"]`            |
| `privateFaxNumbers`          | `array`  | list of new private fax numbers: {"value":"+1234567890"}`                    |
| `businessFaxNumbers`         | `array`  | list of new business fax numbers: `["+41312345678","41318765432"]`           |
| `otherFaxNumbers`            | `array`  | list of new other fax numbers: `["+41312345678","41318765432"]`              |
| `privateUrls`                | `array`  | list of new private urls: `["https://example1.com","https://example2.org"]`  |
| `businessUrls`               | `array`  | list of new business urls: `["https://example1.com","https://example2.org"]` |
| `otherUrls`                  | `array`  | list of new other urls: `["https://example1.com","https://example2.org"]`    |
| `contacts`                   | `array`  | of Hashes `[{"name":"John Doe", "contactAddress":"john@example.com"}]`         |
| &nbsp;&nbsp;`name`           | `string` | name of the contact: `"name":"John Doe"`                                     |
| &nbsp;&nbsp;`contactAddress` | `string` | address/email/phone of the contact: `"contactAddress":"john@example.com"`    |

## Merging strategy

When updating `ContactData`, fields are merged as follows:

* If you set a field, the existing entries in the given field will be replaced.
* If you set a field to `[]`, the existing entries in the given field are deleted.
* If you omit a field, the existing entries in the given field are left in place.

## Type Example

```json
"contactData": {
  "mobilePhoneNumbers": [ "+41791234567", "+41798765432"], # data to update in 'mobilePhoneNumbers' (replaces all existing data)
  "privateEmails": [], # deletes all existing 'privateEmails' data
  "contacts": [
    {"name": "John Doe", "contactAddress": "john.doe@example.com"},
    {"name": "Jane Doe", "contactAddress": "jane.doe@example.com"}
  ]
}
```

**NOTE**: Previously we accepted data inconsistent with our described format.
_Other format are **deprecated** and will be removed in a future release._

### Full Message Example 

to update contact data and document delivery preferences you can send a message with user's contact changes and document delivery preferences:

```json
{
  "eventType": "Masterdata.Person.Update",
  "data": {
    "personReference": "100150",
    "modeOfDispatch": "email",
    "sendEmail": "john@example.com",
    "contactData": {
      "mobilePhoneNumbers": [ "+41791234567", "+41798765432" ],
      "privateEmails": [],
      "contacts": [
        {"name": "John Doe", "contactAddress": "john.doe@example.com"},
        {"name": "Jane Doe", "contactAddress": "+41791234567"}
      ]
    }
  }
}
```

**NOTE**: `sendEmail` is the email address to which the document should be sent when `modeOfDispatch` is set to `email`.
