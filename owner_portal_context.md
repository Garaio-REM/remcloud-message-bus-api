# Owner Portal Context

All attributes are optional unless noted otherwise in the remarks

## Events

| Type                                                        | GARAIO REM         | REM | Description                                       |
| ----------------------------------------------------------- | ------------------ | --- | ------------------------------------------------- |
| [OwnerPortal.Owner.Registered](#ownerportalownerregistered) | :white_check_mark: | :x: | An owner has been registered on the owner portal  |
| [OwnerPortal.Owner.Onboarded](#ownerportalowneronboarded)   | :white_check_mark: | :x: | An owner has been onboarded on the owner portal   |
| [OwnerPortal.Owner.Offboarded](#ownerportalowneroffboarded) | :white_check_mark: | :x: | An owner has been offboarded on the owner portal  |

### OwnerPortal.Owner.Registered

This message represents a message from an owner portal that is sent when an owner has been registered and an access code has been created.

| Field                          | Type   | Content / Remarks                                                    |
| ------------------------------ | ------ | -------------------------------------------------------------------- |
| `eventType`                    | string | OwnerPortal.Owner.Registered                                         |
| `data`                         | hash   |                                                                      |
| &nbsp;&nbsp;`ownerReference`   | string | reference of the GARAIO REM owner (person); **required**             |
| &nbsp;&nbsp;`onboardingUrl`    | string | Onboarding url which will be sent to the owner; **required**         |
| &nbsp;&nbsp;`token`            | string | Optional registration token for authentication                       |
| &nbsp;&nbsp;`validUntil`       | string | Optional expiration date for the registration (ISO 8601 format)      |

#### Example

```json
{"eventType":"OwnerPortal.Owner.Registered",
  "data":{
    "ownerReference":"1234",
    "onboardingUrl":"https://ownerportal.example.com/onboard",
    "token":"abc123-registration-token",
    "validUntil":"2025-12-31"
  }
}
```

#### OwnerPortal.Owner.RegisteredAccepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field            | Type   | Content / Remarks                                        |
| ---------------- | ------ | -------------------------------------------------------- |
| `ownerReference` | string | reference of the GARAIO REM owner (person); **required** |

#### OwnerPortal.Owner.RegisteredRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field            | Type   | Content / Remarks                                        |
| ---------------- | ------ | -------------------------------------------------------- |
| `ownerReference` | string | reference of the GARAIO REM owner (person); **required** |

**Rejection Reasons:**

| Reason | Description |
|--------|-------------|
| `FEATURE_DISABLED` | The eigentuemerportal feature is not enabled |
| `ALREADY_ONBOARDED` | The person is already onboarded for the same app_id |
| `OWNER_ALREADY_REGISTERED` | The person is already registered for a different owner portal |

### OwnerPortal.Owner.Onboarded

This message represents a message from an owner portal that is sent when an owner has been onboarded

| Field                        | Type   | Content / Remarks                                        |
| ---------------------------- | ------ | -------------------------------------------------------- |
| `eventType`                  | string | OwnerPortal.Owner.Onboarded                              |
| `data`                       | hash   |                                                          |
| &nbsp;&nbsp;`ownerReference` | string | reference of the GARAIO REM owner (person); **required** |

#### Example

```json
{"eventType":"OwnerPortal.Owner.Onboarded",
  "data":{
    "ownerReference":"1234"
  }
}
```

#### OwnerPortal.Owner.OnboardedAccepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field            | Type   | Content / Remarks                                        |
| ---------------- | ------ | -------------------------------------------------------- |
| `ownerReference` | string | reference of the GARAIO REM owner (person); **required** |

#### OwnerPortal.Owner.OnboardedRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field            | Type   | Content / Remarks                                        |
| ---------------- | ------ | -------------------------------------------------------- |
| `ownerReference` | string | reference of the GARAIO REM owner (person); **required** |

### OwnerPortal.Owner.Offboarded

This message represents a message from an owner portal that is sent when an owner has been offboarded

| Field                        | Type   | Content / Remarks                                        |
| ---------------------------- | ------ | -------------------------------------------------------- |
| `eventType`                  | string | OwnerPortal.Owner.Offboarded                             |
| `data`                       | hash   |                                                          |
| &nbsp;&nbsp;`ownerReference` | string | reference of the GARAIO REM owner (person); **required** |

#### Example

```json
{"eventType":"OwnerPortal.Owner.Offboarded",
  "data":{
    "ownerReference":"1234"
  }
}
```

#### OwnerPortal.Owner.OffboardedAccepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field            | Type   | Content / Remarks                                        |
| ---------------- | ------ | -------------------------------------------------------- |
| `ownerReference` | string | reference of the GARAIO REM owner (person); **required** |

#### OwnerPortal.Owner.OffboardedRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field            | Type   | Content / Remarks                                        |
| ---------------- | ------ | -------------------------------------------------------- |
| `ownerReference` | string | reference of the GARAIO REM owner (person); **required** |
