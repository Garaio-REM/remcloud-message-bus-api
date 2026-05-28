# Notification Context

## Events

Type                                                        | GARAIO REM         | REM | Description
------------------------------------------------------------|--------------------|-----|-------------------------------------------------------
[Notification.Message.Created](#notificationmessagecreated) | :white_check_mark: | :x: | A notification message has been created
[Notification.Case.Closed](#notificationcaseclosed)         | :white_check_mark: | :x: | The case related to the externalReference has been closed

### Notification.Message.Created

This message represents a message from a messaging participant to a GARAIO REM instance. It is critical that sender (app_id) is set property in the headers - messages need to this info for proper Notification Catigorization. See message the headers reference for more details.

**Field**                           | **Type** | **Content / Remarks**
------------------------------------|----------|-----------------------
eventType                           | string   | `Notification.Message.Created`
data                                | hash     |
&nbsp;&nbsp;externalReference       | string   | external, unique case identifier; **required**
&nbsp;&nbsp;mimetype                | string   | mimetype describing the message format (`text/plain`, `text/markdown`…); **required**
&nbsp;&nbsp;message                 | string   | notification message; **required**
&nbsp;&nbsp;masterdataReference     | string   | reference of a property / building / unit; **required** when `recipientUsername` and `recipientResponsibility` are both absent, and also **required** for any role-based `recipientResponsibility` (see responsibility table below)
&nbsp;&nbsp;recipientUsername       | string   | username of a specific active GREM user to receive the notification. Must exist and be active — an unknown or inactive username is rejected. **required** if `masterdataReference` is absent
&nbsp;&nbsp;recipientResponsibility | string   | routes the notification to the user currently holding this responsibility (see responsibility table below). Only used when `recipientUsername` is absent. Must resolve to an active user or the message is rejected.
&nbsp;&nbsp;subject                 | string   | notification subject; optional
&nbsp;&nbsp;backlinkUrl             | string   | optional URL to navigate back to the sending system; **when given it should be a complete URL the local browser can resolve (including protocol), e.g. `https://www.google.com`**
&nbsp;&nbsp;categoryCode            | string   | optional category code — must match a `code` within the `notification_kategorie` of the CodeTabelle. When blank, the category is resolved dynamically at display time from the sending system's configuration (we use the `app_id` to determine the correct category)
&nbsp;&nbsp;sender                  | string   | optional sender info (email address, name…)

#### `recipientResponsibility` values

Responsibilities come in two kinds, which differ in whether a `masterdataReference` is required:

**Role-based** — the responsible person is the user currently assigned to a specific role on the property identified by the `masterdataReference`. A `masterdataReference` is **required** for these; without one the message is rejected.

Responsibility      | Description
--------------------|-------------------------
ASSISTANT           | The property assistant
PROPERTY_MANAGER    | The property manager (Bewirtschafter)
PROPERTY_ACCOUNTANT | The property accountant (LIBU)
MARKETPLACE_MANAGER | The marketplace manager
LETTING_MANAGER     | The letting manager

**User-based** — the responsible person is a system-level user configured in system parameters, not tied to any specific property. A `masterdataReference` is **ignored** for these; it may be present in the message but has no effect on recipient resolution.

Responsibility            | Description
--------------------------|---------------------------
DATA_PROTECTION_OFFICER   | The configured data protection officer
OWNER_PORTAL_CONTACT      | The configured owner portal contact
MARKETPLACE_EMAIL_MANAGER | The configured marketplace email manager

#### Recipient resolution

A notification message must supply enough information to resolve exactly one active recipient. The following three paths are tried based on what fields are present. If the chosen path cannot resolve an active recipient the message is **rejected** — there is no cross-path fallback.

**Path 1 — `recipientUsername` is provided**

The named user must exist in GREM and be active. There is no fallback: an unknown or inactive username always produces an error. `masterdataReference` and `recipientResponsibility` are ignored for recipient resolution (though `masterdataReference` is still stored on the notification if provided).

**Path 2 — `recipientResponsibility` is provided (without `recipientUsername`)**

The responsibility must be known, configured in system parameters, and currently resolve to an active user. For role-based responsibilities a `masterdataReference` is also required. There is no fallback: a responsibility that cannot be resolved always produces an error.

**Path 3 — `masterdataReference` only (neither `recipientUsername` nor `recipientResponsibility`)**

The recipient is resolved via the following fallback chain, in order:

1. The current **Assistent** for the property
2. The first active user found among the manager roles, checked in this order: `PROPERTY_MANAGER` → `PROPERTY_ACCOUNTANT` → `MARKETPLACE_MANAGER` → `LETTING_MANAGER`

If none of these resolve to an active user the message is rejected.

**Path 4 — nothing provided**

Rejected immediately. At minimum a `recipientUsername` or a `masterdataReference` is required.

#### Examples

Minimal message using `recipientUsername`:

```json
{
  "eventType": "Notification.Message.Created",
  "data": {
    "externalReference": "1234",
    "recipientUsername": "bookkeeper_wht",
    "mimetype": "text/plain",
    "message": "something went wrong"
  }
}
```

Minimal message using `masterdataReference` (recipient resolved via fallback chain — assistent first, then manager roles):

```json
{
  "eventType": "Notification.Message.Created",
  "data": {
    "externalReference": "1234",
    "masterdataReference": "4712.01.0001",
    "mimetype": "text/plain",
    "message": "something happened"
  }
}
```

Message with a role-based `recipientResponsibility` (requires `masterdataReference`):

```json
{
  "eventType": "Notification.Message.Created",
  "data": {
    "externalReference": "1234",
    "masterdataReference": "4712.01.0001",
    "recipientResponsibility": "LETTING_MANAGER",
    "mimetype": "text/plain",
    "message": "something happened"
  }
}
```

Message with a user-based `recipientResponsibility` (`masterdataReference` not required):

```json
{
  "eventType": "Notification.Message.Created",
  "data": {
    "externalReference": "1234",
    "recipientResponsibility": "DATA_PROTECTION_OFFICER",
    "mimetype": "text/plain",
    "message": "something happened"
  }
}
```

Typical full message:

```json
{
  "eventType": "Notification.Message.Created",
  "data": {
    "externalReference": "1234",
    "masterdataReference": "4712.01.0001",
    "sender": "max.muster@mailprovider.com",
    "mimetype": "text/plain",
    "message": "something happened",
    "backlinkUrl": "https://instance.external_system.ch/case/3"
  }
}
```

---

### Notification.Case.Closed

This message notifies GARAIO REM that the case related to the `externalReference` has been closed. All notifications for that reference will be deleted. Set the `recipient` property in the message headers (e.g. `grem_derham`).

Field                         | Type   | Content / Remarks
------------------------------|--------|-------------------------
eventType                     | string | `Notification.Case.Closed`
data                          | hash   |
&nbsp;&nbsp;externalReference | string | external, unique case identifier; **required**

#### Example

```json
{
  "eventType": "Notification.Case.Closed",
  "data": {
    "externalReference": "1234"
  }
}
```
