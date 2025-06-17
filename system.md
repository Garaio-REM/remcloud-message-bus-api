# System Context

## Events

| Type                                      | GARAIO REM         | REM | Description                                                                          |
| ----------------------------------------- | ------------------ | --- | ------------------------------------------------------------------------------------ |
| [System.User.Created](#systemusercreated) | :white_check_mark: | :x: | A user has been created; you get the reference of the created user                   |
| [System.User.Updated](#systemuserupdated) | :white_check_mark: | :x: | Data associated with the user has changed; you get the reference of the changed user |
| [System.User.Deleted](#systemuserdeleted) | :white_check_mark: | :x: | A user was deleted; you get the reference of the deleted user                        |

### System.User.Created

Field | Type | Content / Remarks
---|---|---
eventType | string | System.User.Created
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the user (username)

#### Example

```json
{"eventType":"System.User.Created",
  "data":{
    "reference":"username"
  }
}
```

### System.User.Updated

Field | Type | Content / Remarks
---|---|---
eventType | string | System.User.Updated
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the user (username)

#### Example

```json
{"eventType":"System.User.Updated",
  "data":{
    "reference":"username"
  }
}
```

### System.User.Deleted

Field | Type | Content / Remarks
---|---|---
eventType | string | System.User.Deleted
data | hash |
&nbsp;&nbsp;reference | string | unique identifier for the user (username)

#### Example

```json
{"eventType":"System.User.Deleted",
  "data":{
    "reference":"username"
  }
}
```
