# Tenant Portal Context
## Events

Events in this context are always fired for a single grem instance. The [recipient](/header_properties.md/#AdditionalHeaderProperties) header property must be set to the grem instance name in order to route the event to the customer, e.g. recipient: grem_demo1

Type | GARAIO REM | REM | Description
---|---|---|---
[`TenantPortal.Tenant.Registered`](#tenantportaltenantregistered) | :white_check_mark: | :x: | A tenant has been registered in the tenant portal and an access code has been created


### TenantPortal.Tenant.Registered

Field | Type | Content / Remarks
---|---|---
`eventType` | `string` | TenantPortal.Tenant.Registered
`data` | `hash` |
&nbsp;&nbsp;`tenantReference` | `string` | unique person identifier, eg `'1234'` |
&nbsp;&nbsp;`unitReference` | `string` | String referencing an existing unit in the target GARAIO REM |
&nbsp;&nbsp;`registrationCode` | `string` | Registration code coming from the TenantPortal |
&nbsp;&nbsp;`onboardingUrl` | `string` | Onboarding url which will be sent to the tenant |

#### Example

```json
{
  "eventType":"TenantPortal.Tenant.Registered",
  "data":{
    "tenantReference":"100004",
    "unitReference":"1234.01.0001",
    "registrationCode":"A1234B99",
    "onboardingUrl":"https://mieterportal.garaio.com"
  }
}
```
