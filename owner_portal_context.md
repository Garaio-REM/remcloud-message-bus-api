# Owner Portal Context

## Events

| Type                                                        | GARAIO REM         | REM | Description                                      |
| ----------------------------------------------------------- | ------------------ | --- | ------------------------------------------------ |
| [OwnerPortal.Owner.Onboarded](#ownerportalowneronboarded)   | :white_check_mark: | :x: | An owner has been onboarded on the owner portal  |
| [OwnerPortal.Owner.Offboarded](#ownerportalowneroffboarded) | :white_check_mark: | :x: | An owner has been offboarded on the owner portal |

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
