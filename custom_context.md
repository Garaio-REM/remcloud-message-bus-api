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

#### Custom.EntityData.UpdateRejected

The [Reject](./result_messages.md#rejected-message) message.

Additional `data` fields:

| Field             | Type      | Content / Remarks                             |
| ----------------- | --------- | --------------------------------------------- |
| `referencedTable` | `string`  | Value given in the `Update` message           |
| `reference`       | `string`  | Value given in the `Update` message, if given |
| `id`              | `integer` | Value given in the `Update` message, if given |
