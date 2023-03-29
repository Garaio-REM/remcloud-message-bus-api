# Property Check Context

## Events

Type | GARAIO REM | REM | Description
---|---|---|---
[PropertyCheck.Check.Published](#propertycheckpublished) | :white_check_mark: | :x: | A Property Check has been published

### PropertyCheck.Check.Published

This message goes from the property check provider to GARAIO REM. Set the recipient/app_id property in the headers, eg `"grem_derham"`. All attributes are optional unless noted otherwise in the remarks

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | `DamageCase.Case.Published`
`data` | `hash` |
&nbsp;&nbsp;`reference` | `string` | identifier for the published property check; **required**

#### Example

```json
{"eventType":"PropertyCheck.Check.Published",
  "data":{
    "reference":"1234"
  }
}
```
