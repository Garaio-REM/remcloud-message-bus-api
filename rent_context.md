# Rent Context

## Events

| Type | GARAIO REM | REM | Description |
|---|---|---|---|
| [Rent.Rent.Created](#rentrentcreated) | :white_check_mark: | :x: | A new rent configuration has been created |
| [Rent.Rent.Updated](#rentrentupdated) | :white_check_mark: | :x: | Data associated to a rent configuration has changed |
| [Rent.Rent.Deleted](#rentrentdeleted) | :white_check_mark: | :x: | A rent configuration was deleted |
| [Rent.RentIncreasePotential.Created](#rentrentincreasepotentialcreated) | :white_check_mark: | :x: | A new rent reserve has been created |
| [Rent.RentIncreasePotential.Deleted](#rentrentincreasepotentialdeleted) | :white_check_mark: | :x: | A rent reserve was deleted |
| [Rent.TheoreticalRent.Created](#renttheoreticalrentcreated) | :white_check_mark: | :x: | A new theoretical rent has been created |
| [Rent.TheoreticalRent.Deleted](#renttheoreticalrentdeleted) | :white_check_mark: | :x: | A theoretical rent was deleted |
| [Rent.PublishedRentalCost.Created](#rentpublishedrentalcostcreated) | :white_check_mark: | :x: | A new published rental cost has been created |
| [Rent.PublishedRentalCost.Updated](#rentpublishedrentalcostupdated) | :white_check_mark: | :x: | Data associated to a published rental cost has changed |
| [Rent.PublishedRentalCost.Deleted](#rentpublishedrentalcostdeleted) | :white_check_mark: | :x: | A published rental cost was deleted |

Note: Updates on RentIncreasePotential and TheoreticalRent messages are published as a Delete and a Create event.

### Rent.Rent.Created

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.Rent.Created |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this rent belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date when this rent configuration becomes valid |
| &nbsp;&nbsp;approved | boolean | whether the rent is approved |
| &nbsp;&nbsp;netRent | number | net rent amount |
| &nbsp;&nbsp;totalArea | number | total area in m² |
| &nbsp;&nbsp;effectiveArea | number | effective/usable area in m² |
| &nbsp;&nbsp;areaTypeCode | string | code indicating the type of area measurement used |
| &nbsp;&nbsp;totalVolume | number | total volume in m³ |
| &nbsp;&nbsp;effectiveVolume | number | effective/usable volume in m³ |
| &nbsp;&nbsp;numberOfUnits | integer | number of units this rent applies to |
| &nbsp;&nbsp;components | array | array of rent components |
| &nbsp;&nbsp;&nbsp;&nbsp;type | string | component type code |
| &nbsp;&nbsp;&nbsp;&nbsp;amount | number | net amount of the component |

#### Example

```json
{
  "eventType": "Rent.Rent.Created",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01",
    "approved": true,
    "netRent": 1500.00,
    "totalArea": 75.5,
    "effectiveArea": 70.0,
    "areaTypeCode": "SIA416",
    "totalVolume": 226.5,
    "effectiveVolume": 210.0,
    "numberOfUnits": 1,
    "components": [
      {
        "type": "NET_RENT",
        "amount": 1500.00
      }
    ]
  }
}
```

### Rent.Rent.Updated

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.Rent.Updated |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this rent belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date when this rent configuration becomes valid |
| &nbsp;&nbsp;netRent | number | net rent amount (if changed) |
| &nbsp;&nbsp;totalArea | number | total area in m² (if changed) |
| &nbsp;&nbsp;effectiveArea | number | effective/usable area in m² (if changed) |
| &nbsp;&nbsp;areaTypeCode | string | code indicating the type of area measurement used (if changed) |
| &nbsp;&nbsp;totalVolume | number | total volume in m³ (if changed) |
| &nbsp;&nbsp;effectiveVolume | number | effective/usable volume in m³ (if changed) |
| &nbsp;&nbsp;numberOfUnits | integer | number of units this rent applies to (if changed) |
| &nbsp;&nbsp;components | array | array of rent components (if changed) |
| &nbsp;&nbsp;&nbsp;&nbsp;type | string | component type code |
| &nbsp;&nbsp;&nbsp;&nbsp;amount | number | net amount of the component |

#### Example

```json
{
  "eventType": "Rent.Rent.Updated",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01",
    "netRent": 1600.00,
    "components": [
      {
        "type": "NET_RENT",
        "amount": 1600.00
      }
    ]
  }
}
```

### Rent.Rent.Deleted

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.Rent.Deleted |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this rent belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date of the rent configuration that was deleted |

#### Example

```json
{
  "eventType": "Rent.Rent.Deleted",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01"
  }
}
```

### Rent.RentIncreasePotential.Created

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.RentIncreasePotential.Created |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this rent increase potential belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date when this rent increase potential becomes valid |
| &nbsp;&nbsp;approved | boolean | whether the rent increase potential is approved |
| &nbsp;&nbsp;amount | decimal | amount of the rent increase potential |
| &nbsp;&nbsp;note | string | additional note or remark about the rent increase potential |
| &nbsp;&nbsp;typeCode | string | code indicating the type of rent increase potential; (see code table entries (`mietzinsreserve_typ`) for valid codes) |

#### Example

```json
{
  "eventType": "Rent.RentIncreasePotential.Created",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01",
    "approved": true,
    "amount": 500.00,
    "note": "Annual rent reserve",
    "typeCode": "STANDARD"
  }
}
```

### Rent.RentIncreasePotential.Deleted

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.RentIncreasePotential.Deleted |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this rent increase potential belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date of the rent increase potential that was deleted |
| &nbsp;&nbsp;typeCode | string | code indicating the type of rent increase potential; (see code table entries (`mietzinsreserve_typ`) for valid codes) |

#### Example

```json
{
  "eventType": "Rent.RentIncreasePotential.Deleted",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01",
    "typeCode": "STANDARD"
  }
}
```

### Rent.TheoreticalRent.Created

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.TheoreticalRent.Created |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this theoretical rent belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date when this theoretical rent becomes valid |
| &nbsp;&nbsp;approved | boolean | whether the theoretical rent is approved |
| &nbsp;&nbsp;amount | decimal | amount of the theoretical rent |
| &nbsp;&nbsp;note | string | additional note or remark about the theoretical rent |
| &nbsp;&nbsp;typeCode | string | code indicating the type of theoretical rent; (see code table entries (`theoretischer_mietzins_typ`) for valid codes) |

#### Example

```json
{
  "eventType": "Rent.TheoreticalRent.Created",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01",
    "approved": true,
    "amount": 500.00,
    "note": "Auf Wunsch ET",
    "typeCode": "STANDARD"
  }
}
```

### Rent.TheoreticalRent.Deleted

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.TheoreticalRent.Deleted |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this theoretical rent belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date of the theoretical rent that was deleted |
| &nbsp;&nbsp;typeCode | string | code indicating the type of theoretical rent; (see code table entries (`theoretischer_mietzins_typ`) for valid codes) |

#### Example

```json
{
  "eventType": "Rent.TheoreticalRent.Deleted",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01",
    "typeCode": "STANDARD"
  }
}
```

### Rent.PublishedRentalCost.Created

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.PublishedRentalCost.Created |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this published rental cost belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date when this published rental cost becomes valid |
| &nbsp;&nbsp;approved | boolean | whether the published rental cost is approved |
| &nbsp;&nbsp;netRent | number | net rent amount |
| &nbsp;&nbsp;totalArea | number | total area in m² |
| &nbsp;&nbsp;effectiveArea | number | effective/usable area in m² |
| &nbsp;&nbsp;areaTypeCode | string | code indicating the type of area measurement used |
| &nbsp;&nbsp;totalVolume | number | total volume in m³ |
| &nbsp;&nbsp;effectiveVolume | number | effective/usable volume in m³ |
| &nbsp;&nbsp;numberOfUnits | integer | number of units this rent applies to |

#### Example

```json
{
  "eventType": "Rent.PublishedRentalCost.Created",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01",
    "approved": true,
    "netRent": 1500.00,
    "totalArea": 75.5,
    "effectiveArea": 70.0,
    "areaTypeCode": "SIA416",
    "totalVolume": 226.5,
    "effectiveVolume": 210.0,
    "numberOfUnits": 1
  }
}
```

### Rent.PublishedRentalCost.Updated

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.PublishedRentalCost.Updated |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this published rental cost belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date when this published rental cost becomes valid |
| &nbsp;&nbsp;netRent | number | net rent amount (if changed) |
| &nbsp;&nbsp;totalArea | number | total area in m² (if changed) |
| &nbsp;&nbsp;effectiveArea | number | effective/usable area in m² (if changed) |
| &nbsp;&nbsp;areaTypeCode | string | code indicating the type of area measurement used (if changed) |
| &nbsp;&nbsp;totalVolume | number | total volume in m³ (if changed) |
| &nbsp;&nbsp;effectiveVolume | number | effective/usable volume in m³ (if changed) |
| &nbsp;&nbsp;numberOfUnits | integer | number of units this rent applies to (if changed) |

#### Example

```json
{
  "eventType": "Rent.PublishedRentalCost.Updated",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01",
    "netRent": 1600.00,
    "totalArea": 75.5,
    "effectiveArea": 70.0,
    "areaTypeCode": "SIA416",
    "totalVolume": 226.5,
    "effectiveVolume": 210.0,
    "numberOfUnits": 1
  }
}
```

### Rent.PublishedRentalCost.Deleted

| Field | Type | Content / Remarks |
|---|---|---|
| eventType | string | Rent.PublishedRentalCost.Deleted |
| data | hash | |
| &nbsp;&nbsp;unitReference | string | unique identifier for the unit this published rental cost belongs to |
| &nbsp;&nbsp;validFrom | string | ISO 8601 encoded date of the published rental cost that was deleted |

#### Example

```json
{
  "eventType": "Rent.PublishedRentalCost.Deleted",
  "data": {
    "unitReference": "1234.01.0001",
    "validFrom": "2025-04-01"
  }
}
```
