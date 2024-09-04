# Condominium Context

## Events

Type | GARAIO REM | REM | Description
---|---|---|---
[Condominium.Ownership.Created](#condominiumownershipcreated) | :white_check_mark: | :x: | A new condominium has been created
[Condominium.Ownership.Deleted](#condominiumownershipdeleted) | :white_check_mark: | :x: | A condominium has been deleted
[Condominium.Ownership.Updated](#condominiumownershipupdated) | :white_check_mark: | :x: | A condominium has been updated

### Condominium.Ownership.Created

Field | Type | Content / Remarks
---|---|---
`eventType` | string | `Condominium.Ownership.Created`
`data` | hash |
&nbsp;&nbsp;`unitReference` | string | Unique identifier for the unit
&nbsp;&nbsp;`startDate` | string | ISO 8601 encoded date, e.g., '2024-04-13'
&nbsp;&nbsp;`owner` | hash |
&nbsp;&nbsp;&nbsp;&nbsp;`reference` | string | Unique identifier for the owner
&nbsp;&nbsp;&nbsp;&nbsp;`languageCode` | string | Language code, e.g., 'de'
&nbsp;&nbsp;&nbsp;&nbsp;`nationalityCode` | string | Nationality code, might be null
&nbsp;&nbsp;&nbsp;&nbsp;`phoneNumber` | string | Phone number, might be null
&nbsp;&nbsp;&nbsp;&nbsp;`email` | string | Email address, might be null
&nbsp;&nbsp;&nbsp;&nbsp;`salutationLines` | array | Array of salutation lines
&nbsp;&nbsp;&nbsp;&nbsp;`nameLines` | array | Array of name lines
&nbsp;&nbsp;&nbsp;&nbsp;`addressLines` | array | Array of address lines

#### Example

```json
{
  "eventType": "Condominium.Ownership.Created",
  "data": {
    "unitReference": "10001.693.891",
    "startDate": "2024-09-04",
    "owner": {
      "reference": "100004",
      "languageCode": "de",
      "nationalityCode": null,
      "phoneNumber": null,
      "email": null,
      "salutationLines": [
        "Sehr geehrte Familie Meier"
      ],
      "nameLines": [
        "Hans Meier",
        "Erika Meier"
      ],
      "addressLines": [
        "Herr Hans Meier",
        "Musterstrasse 1",
        "1234 Musterstadt"
      ]
    }
  }
}
```

### Condominium.Ownership.Deleted

Field | Type | Content / Remarks
---|---|---
`eventType` | string | `Condominium.Ownership.Deleted`
`data` | hash |
&nbsp;&nbsp;`startDate` | string | ISO 8601 encoded date, e.g., '2023-04-13'
&nbsp;&nbsp;`unitReference` | string | Unique identifier for the unit
&nbsp;&nbsp;`ownerReference` | string | Unique identifier for the owner

#### Example

```json
{
  "eventType": "Condominium.Ownership.Deleted",
  "data": {
    "startDate": "2023-09-04",
    "unitReference": "10001.184.88",
    "ownerReference": "100004"
  }
}
```

### Condominium.Ownership.Updated

Field | Type | Content / Remarks
---|---|---
`eventType` | string | `Condominium.Ownership.Updated`
`data` | hash |
&nbsp;&nbsp;`unitReference` | string | Unique identifier for the unit
&nbsp;&nbsp;`startDate` | string | ISO 8601 encoded date, e.g., '2024-04-13'
&nbsp;&nbsp;`ownerReference` | string | Unique identifier for the owner

#### Example

```json
{
  "eventType": "Condominium.Ownership.Updated",
  "data": {
    "unitReference": "10001.469.650",
    "startDate": "2024-09-04",
    "ownerReference": "100004"
  }
}
