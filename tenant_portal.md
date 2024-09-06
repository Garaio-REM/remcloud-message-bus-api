# Tenant Portal Context
## Events

Events in this context are always fired for a single grem instance. The [recipient](/header_properties.md/#AdditionalHeaderProperties) header property must be set to the grem instance name in order to route the event to the customer, e.g. recipient: grem_demo1

Type | GARAIO REM | REM | Description
---|---|---|---
[`TenantPortal.Tenant.Registered`](#tenantportaltenantregistered) | :white_check_mark: | :x: | A tenant has been registered in the tenant portal and an access code has been created
[`TenantPortal.Tenant.Accepted`](#tenantportaltenantaccepted) | :white_check_mark: | :x: | The tenant registration in the tenant portal has been accepted by GARAIO REM
[`TenantPortal.Tenant.Rejected`](#tenantportaltenantrejected) | :white_check_mark: | :x: | The tenant registration in the tenant portal has been rejected by GARAIO REM (with reasons for rejection)


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

### TenantPortal.Tenant.Accepted

This message goes from GARAIO REM to the portal and signals that the tenant has been accepted by Garai REM.


| Field                         |	Type     |	Content / Remarks                                           |
|-------------------------------|----------|--------------------------------------------------------------|
| `eventType`	                  | `string` | Invoicing.Order.Accepted                                     |
| `data`                      	| `hash`   |                                                              |
| &nbsp;&nbsp;`tenantReference` | `string` | unique person identifier, eg `'1234'`                        |
| &nbsp;&nbsp;`unitReference`   | `string` | String referencing an existing unit in the target GARAIO REM |

#### Example
```json
{"eventType":"Invoicing.Order.Accepted",
  "data":{
    "tenantReference":"1234",
    "unitReference":"1234.01.0001"
  }
}
```

### TenantPortal.Tenant.Rejected

This message goes from GARAIO REM to the portal and signals that the tenant has been rejected by Garai REM. GARAIO REM validation errors are mapped into the reasons array.

| Field                               |	Type     |	Content / Remarks                                           |
|-------------------------------------|----------|--------------------------------------------------------------|
| `eventType`	                        | `string` | Invoicing.Order.Rejected                                     |
| `data`	                            | `hash`   |                                                              |
| &nbsp;&nbsp;`tenantReference`       | `string` | unique person identifier, eg `'1234'`                        |
| &nbsp;&nbsp;`unitReference`         | `string` | String referencing an existing unit in the target GARAIO REM |
| &nbsp;&nbsp;`reasons`               | `array`  |                                                              |
| &nbsp;&nbsp;&nbsp;&nbsp;`attribute` | `string` | name of the attribute, eg. "masterdataReference"; (1)        |
| &nbsp;&nbsp;&nbsp;&nbsp;`lineNumber`| `string` | line number, eg. "1" (or null)                               |
| &nbsp;&nbsp;&nbsp;&nbsp;`reason`    | `string` | reason, eg. "ist nicht bekannt"                              |

**Notes:**

* (1) might be null if the reason does not map to a specific attribute

#### Example
```json
{"eventType":"Invoicing.Order.Rejected",
  "data":{
    "tenantReference":"1234",
    "unitReference":"1234.01.0001",
    "reasons":[
      {"attribute":"tenantReference",
       "lineNumber":null,
       "reason":"ungültige Person-Referenz: invalid'"
      }
    ]
  }
}
```
