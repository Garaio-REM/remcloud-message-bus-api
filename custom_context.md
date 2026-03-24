# Custom

Management of customer specific data inside GREM.

## Events

| Type                                                | GARAIO REM         | REM | Description                              |
| --------------------------------------------------- | ------------------ | --- | ---------------------------------------- |
| [Custom.EntityData.Update](#customentitydataupdate) | :white_check_mark: | :x: | Manage custom data for a business entity |

### Custom.EntityData.Update

This message is sent by an external system to GARAIO REM. Manages custom table data for the given entity. See [custom tables](./custom_tables.md) for an explanation of the concept.

| Field                      | Type      | Content / Remarks                                                                                              |
| -------------------------- | --------- | -------------------------------------------------------------------------------------------------------------- |
| `eventType`                | `string`  | `Custom.EntityData.Update`                                                                                     |
| `data`                     | `hash`    |                                                                                                                |
| `referencedTable`          | `string`  | Specifies the database table of the business entity whose data you want to manage. E.g. 'objekte' **required** |
| &nbsp;&nbsp;`reference`    | `string`  | Specifies the reference (value of column `referenz`) of the business entity whose data you want to manage.     |
| &nbsp;&nbsp;`id`           | `integer` | Specifies the id (value of column `id`) of the business entity whose data you want to manage.    (1) (2)       |
| &nbsp;&nbsp;`customTables` | `hash`    | Custom table data, see [custom tables](./custom_tables.md) for a detailed explanation                          |

Notes

* (1) Either `reference` or `id` is required.
* (2) We suggest using `reference` as the identifier. See [Using ids instead of references](./custom_tables.md#using-ids-instead-of-references) for caveats and detailed reasoning.

#### Example

```json
{
  "eventType": "Custom.EntityData.Update",
  "data": {
    "referencedTable": "verwaltungseinheiten",
    "reference": "200601",
    "customTables": {
      "verwaltungseinheit_allgemein": {
        "baujahr": "ca. 1980"
      }
    }
  }
}
```

#### Custom.EntityData.UpdateAccepted

The [Accept](./result_messages.md#accepted-message) message.

Additional `data` fields:

| Field             | Type      | Content / Remarks                             |
| ----------------- | --------- | --------------------------------------------- |
| `referencedTable` | `string`  | Value given in the `Update` message           |
| `reference`       | `string`  | Value given in the `Update` message, if given |
| `id`              | `integer` | Value given in the `Update` message, if given |
| `customTables`    | `hash`    | Each key is a [custom table](./custom_tables.md) name. The value is a hash with `created`, `updated` and `deleted` arrays containing the full row data for each affected record |

The `customTables` hash in the Accepted response groups the affected records by operation:

| Field     | Type    | Content / Remarks                                      |
| --------- | ------- | ------------------------------------------------------ |
| `created` | `array` | Records that were newly inserted (includes assigned `id` and all column values) |
| `updated` | `array` | Records that were modified (includes all current column values) |
| `deleted` | `array` | Records that were removed (includes all column values as they were before deletion) |

##### Example

Given a request that inserts, updates and deletes a record each:

```json
{
  "eventType": "Custom.EntityData.Update",
  "data": {
    "referencedTable": "buchhaltungen",
    "reference": "01234",
    "customTables": {
      "buchhaltung_konten_n": [
        {
          "number": "111",
          "description": "TEST",
        },
        {
          "id": 1234,
          "number": "222"
        },
        {
          "id": 2345,
          "_delete": true
        }
      ]
    }
  }
}
```

The Accepted response:

```json
{
  "eventType": "Custom.EntityData.UpdateAccepted",
  "data": {
    "referencedTable": "buchhaltungen",
    "reference": "01234",
    "customTables": {
      "buchhaltung_konten_n": {
        "created": [
          {
            "id": 3456,
            "buchhaltung_id": 3333,
            "number": "111",
            "description": "TEST inserted record",
            "lock_version": 0,
            "updated_at": "2025-07-29T01:23:45.678+02:00",
            "created_at": "2025-07-29T01:23:45.678+02:00"
          }
        ],
        "updated": [
          {
            "id": 1234,
            "buchhaltung_id": 3333,
            "number": "222",
            "description": "TEST updated record",
            "lock_version": 1,
            "updated_at": "2025-07-29T01:23:45.678+02:00",
            "created_at": "2025-07-29T01:23:45.678+02:00"
          }
        ],
        "deleted": [
          {
            "id": 2345,
            "buchhaltung_id": 3333,
            "number": "333",
            "description": "TEST deleted record",
            "lock_version": 0,
            "updated_at": "2025-07-29T01:23:45.678+02:00",
            "created_at": "2025-07-29T01:23:45.678+02:00"
          }
        ]
      }
    }
  }
}
```

#### Custom.EntityData.UpdateRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field             | Type      | Content / Remarks                             |
| ----------------- | --------- | --------------------------------------------- |
| `referencedTable` | `string`  | Value given in the `Update` message           |
| `reference`       | `string`  | Value given in the `Update` message, if given |
| `id`              | `integer` | Value given in the `Update` message, if given |
