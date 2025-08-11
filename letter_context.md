# Letter Context

## Events

| Type                                          | GARAIO REM         | REM | Description                                                  |
|-----------------------------------------------| ------------------ | --- |--------------------------------------------------------------|
| [Letters.Letter.Create](#letterslettercreate) | :white_check_mark: | :x: | creates a letter in GREM to send an invitation for a meeting |

### Letters.Letter.Create

| Field                                 | Type     | Content / Remarks                                                                               |
|---------------------------------------|----------|-------------------------------------------------------------------------------------------------|
| `eventType`                           | `string` | `Letters.Letter.Create`                                                                         |
| `data`                                | `hash`   |                                                                                                 |
| &nbsp;&nbsp;`letterTemplateReference` | `string` | template used for generating the letter, Reference of the letter template, **required**         |
| &nbsp;&nbsp;`propertyReference`       | `string` | reference of the administrative unit, **required**                                              |
| &nbsp;&nbsp;`clerkUsername`           | `string` | username of the responsible person, **required**                                                |
| &nbsp;&nbsp;`dueDate`                 | `string` | ISO 8601 encoded due date of the letter, **required**                                           |
| &nbsp;&nbsp;`description`             | `string` | subject or short description of the letter, **optional**                                        |
| &nbsp;&nbsp;`signatureLeft`           | `string` | username for the left-side signature block , **optional**                                       |
| &nbsp;&nbsp;`signatureRight`          | `string` | username for the right-side signature block, **optional**                                       |
| &nbsp;&nbsp;`customAttributes`        | `hash`   | customAttributes that are used in the letterTemplate. E.g. the text of the letter. **optional** |

#### Example

```json
{
  "eventType":"Letters.Letter.Create",
  "data": {
    "letterTemplateReference": "liegenschaft_blanko",
    "clerkUsername": "admgar",
    "dueDate": "2025-07-24",
    "propertyReference": "13000",
    "description": "Serial Condominium Letter ONE UNIT",
    "signatureLeft": "welldev",
    "signatureRight": "garaio.dev",
    "customAttributes": {
      "brieftext": "<p>HTML content of the letter</p>"
    }
  }
}
```

##### accepted response message

```json
{
  "eventType": "Letters.Letter.CreateAccepted", 
  "data": {
    "reference": 212
  }
}
```

| Field | Type     | Content / Remarks                   |
|-------| -------- |-------------------------------------|
| `id`  | `string` | the id of the newly created letter. |

##### rejected response message

```json
{
  "eventType": "Letters.Letter.CreateRejected",
  "data": {
    "reasons": [
      {
        "attribute": "dueDate",
        "reason": "ist ungültig"
      }
    ]
  }
}
```
