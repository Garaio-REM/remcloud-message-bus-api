# Damage Case Context

## Events

Type | GARAIO REM | REM | Description
---|---|---|---
[DamageCase.Case.Published](#damagecasecasepublished) | :white_check_mark: | :x: | A Damage Case has been published

### DamageCase.Case.Published

This message goes from the damage case provider to GARAIO REM. Set the recipient/app_id property in the headers, eg `"grem_derham"`. All attributes are optional unless noted otherwise in the remarks

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | `DamageCase.Case.Published`
`data` | `hash` |
&nbsp;&nbsp;`messageReference` | `string` | identifier for the published damage case; **required**
&nbsp;&nbsp;`editorUsername` | `string` | username of the user that published the damage case; **required**

#### Example

```json
{"eventType":"DamageCase.Case.Published",
  "data":{
    "messageReference":"1234",
    "editorUsername":"user@garaio-rem.ch"
  }
}
```
