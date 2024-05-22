# Errors

Errors sent by GREM as a response to invalid messages.

These will occur if your message does not confirm to the basic rules as given in [README.md](./README.md).

## Events

| Type                                            | GARAIO REM         | REM | Description                    |
| ----------------------------------------------- | ------------------ | --- | ------------------------------ |
| [Errors.Message.Invalid](#errorsmessageinvalid) | :white_check_mark: | :x: | A received message was invalid |

### Errors.Message.Invalid

| Field       | Type     | Content / Remarks                             |
| ----------- | -------- | --------------------------------------------- |
| `eventType` | `string` | `Errors.Message.Invalid`                      |
| `data`      | `hash`   |                                               |
| `errors`    | `array`  | An array of strings destailing the errors (1) |

* (1) e.g. "Unknown eventType 'Foo'" or "Property message_id missing for message of type XYZ".