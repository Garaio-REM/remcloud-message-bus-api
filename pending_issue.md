# Pending Issue

Pending Issue messages are meant to be used by customers that run their own services and want to integrate them by managing pending issues. **They are not meant to be used by third party services. These should use [notifications](notification_context.md) instead.**

## Events

| Type                                                  | GARAIO REM         | REM | Description                             |
| ----------------------------------------------------- | ------------------ | --- | --------------------------------------- |
| [PendingIssue.Issue.Create](#pendingissueissuecreate) | :white_check_mark: | :x: | Request creation of a pending issue     |
| [PendingIssue.Issue.Close](#pendingissueissueclose)   | :white_check_mark: | :x: | Request closing of a pending issue      |
| [PendingIssue.Issue.Closed](#pendingissueissueclosed) | :white_check_mark: | :x: | A pending issue has been closed in GREM |

### PendingIssue.Issue.Create

This message is sent by an external message published to GARAIO REM. Set the recipient property in the headers, eg `"grem_derham"`. All attributes are optional unless noted otherwise in the remarks

| Field                           | Type      | Content / Remarks                                                               |
| ------------------------------- | --------- | ------------------------------------------------------------------------------- |
| `eventType`                     | `string`  | `PendingIssue.Issue.Create`                                                     |
| `data`                          | `hash`    |                                                                                 |
| &nbsp;&nbsp;`subject`           | `string`  | Subject **required**                                                            |
| &nbsp;&nbsp;`dueDate`           | `string`  | ISO 8601 encoded date, e.g. `2020-10-21` **required**                           |
| &nbsp;&nbsp;`categoryCode`      | `string`  | Category code (from code table `kategorie`) **required**                        |
| &nbsp;&nbsp;`recipientUsername` | `string`  | Username of the recipient **required**                                          |
| &nbsp;&nbsp;`senderUsername`    | `string`  | Username of the sender (1)                                                      |
| &nbsp;&nbsp;`externalUrl`       | `string`  | URL for the link displayed in GREM, e.g. `https://www.example.com` **required** |
| &nbsp;&nbsp;`targetType`        | `string`  | Class of business object this issue applies to (2)                              |
| &nbsp;&nbsp;`targetReference`   | `string`  | Reference of business object this issue applies to (2)                          |
| &nbsp;&nbsp;`description`       | `string`  | Description                                                                     |
| &nbsp;&nbsp;`externalReference` | `string`  | External reference **required**                                                 |
| &nbsp;&nbsp;`manuallyDeletable` | `boolean` | Whether or not the user can delete the issue in GREM. Defaults to false. (3)    |

Notes

* (1) Defaults to application user for given app_id
* (2) You may pass the class name and the reference of a business object. As of now the following targets are supported:
  * `Buchhaltung`
  * `Buchung`
  * `DauerauftragZahlung`
  * `Haus`
  * `Kreditorenauftrag`
  * `KreditorRechnung`
  * `Mietvertrag`
  * `Objekt`
  * `OffertAnfrage`
  * `Organisationseinheit`
  * `Person`
  * `Textbaustein`
  * `Verwaltungseinheit`
  (3) You'll receive a `PendingIssue.Issue.Closed` when the user closes the issue

#### Example

```json
{
  "eventType": "PendingIssue.Issue.Create",
  "data": {
    "subject": "do this",
    "dueDate": "2025-01-01",
    "categoryCode": "00",
    "recipientUsername": "Testuser_1",
    "senderUsername": "Testuser_2",
    "backlinkUrl": "https://example.com"
  }
}
```

#### PendingIssue.Issue.Accepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field               | Type     | Content / Remarks                                   |
| ------------------- | -------- | --------------------------------------------------- |
| `externalReference` | `string` | The external reference given in the request message |

#### PendingIssue.Issue.Rejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field               | Type     | Content / Remarks                                   |
| ------------------- | -------- | --------------------------------------------------- |
| `externalReference` | `string` | The external reference given in the request message |

### PendingIssue.Issue.Close

This message goes from the order provider to GARAIO REM. Set the recipient property in the headers, eg `"grem_derham"`. All attributes are optional unless noted otherwise in the remarks

| Field                           | Type     | Content / Remarks                     |
| ------------------------------- | -------- | ------------------------------------- |
| `eventType`                     | `string` | `PendingIssue.Issue.Close`            |
| `data`                          | `hash`   |                                       |
| &nbsp;&nbsp;`externalReference` | `string` | External reference  **required**  (1) |

Notes

* (1) As given when creating the pending issue with `PendingIssue.Issue.Create`

#### Example

```json
{
  "eventType": "PendingIssue.Issue.Close",
  "data": {
    "externalReference": "ref"
  }
}
```

#### PendingIssue.Issue.CloseAccepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field               | Type     | Content / Remarks                                   |
| ------------------- | -------- | --------------------------------------------------- |
| `externalReference` | `string` | The external reference given in the request message |

#### PendingIssue.Issue.CloseRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field               | Type     | Content / Remarks                                   |
| ------------------- | -------- | --------------------------------------------------- |
| `externalReference` | `string` | The external reference given in the request message |

### PendingIssue.Issue.Closed

Sent by GREM whenever a message previously created by `PendingIssue.Issue.Close` that has an `externalReference` is closed by the user in GREM.

| Field                           | Type     | Content / Remarks           |
| ------------------------------- | -------- | --------------------------- |
| `eventType`                     | `string` | `PendingIssue.Issue.Closed` |
| `data`                          | `hash`   |                             |
| &nbsp;&nbsp;`externalReference` | `string` | External reference          |

#### Example

```json
{
  "eventType": "PendingIssue.Issue.Closed",
  "data": {
    "externalReference": "ref"
  }
}
```
