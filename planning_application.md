# Planning Application Context

## Events

Events in this context are always fired for a single grem instance. The recipient header property must be set to the grem instance name in order to route the event to the customer, e.g. recipient: grem_demo1

Type | GARAIO REM | REM | Description
---|---|---|---
[PlanningApplication.Application.Submitted](#planningapplicationapplicationsubmitted) | :white_check_mark: | :x: | A planning application was submitted that may affect a building

### PlanningApplication.Application.Submitted

Field | Type | Content / Remarks
---|---|---
eventType | string | PlanningApplication.Application.Submitted
data | hash |
&nbsp;&nbsp;buildingReference | string | String referencing an existing building in the target GARAIO REM |
&nbsp;&nbsp;documentUrl | string | Url where the document can be downloaded |

#### Example

```json
{
  "eventType":"PlanningApplication.Application.Submitted",
  "data":{
    "buildingReference":"1234.01",
    "documentUrl":"https://some_document.pdf"
  }
}
```
