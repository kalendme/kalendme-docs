# Resources

Resources are shared organization assets (e.g. meeting rooms, equipment) that can be booked directly by API consumers, by email (with the generated `emailAlias`), or through an optional public booking page. Each resource has its own weekly availability window and an ACL that controls who can book it.

## Resource Resource

> An example resource model looks like this:

```json
{
  "resource": {
    "id": "r_9f8a1c",
    "organizationId": "org_11223344",
    "name": "Boardroom A",
    "description": "10-seat boardroom on the 4th floor",
    "emailAlias": "boardroom-a",
    "timezone": "America/Sao_Paulo",
    "weekAvailability": {
      "0": [],
      "1": [{ "start": "09:00", "end": "18:00" }],
      "2": [{ "start": "09:00", "end": "18:00" }],
      "3": [{ "start": "09:00", "end": "18:00" }],
      "4": [{ "start": "09:00", "end": "18:00" }],
      "5": [{ "start": "09:00", "end": "18:00" }],
      "6": []
    },
    "acl": { "mode": "org_members" },
    "metadata": {
      "capacity": 10,
      "floor": 4,
      "building": "HQ",
      "equipment": ["projector", "whiteboard"]
    },
    "isEnabled": true,
    "isPubliclyListed": false,
    "feedUrl": "https://www.kalendme.com/api/resources/r_9f8a1c/feed/5f4d...abc1.ics",
    "createdAt": "2026-04-01T12:00:00.000Z",
    "updatedAt": "2026-04-10T08:30:00.000Z"
  }
}
```

| Parameter        | Type                                     | Description                                                                                                                                                                     |
| ---------------- | ---------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id               | string                                   | The resource's id                                                                                                                                                              |
| organizationId   | string                                   | The organization the resource belongs to                                                                                                                                       |
| name             | string                                   | Human-readable resource name (1–50 chars)                                                                                                                                      |
| description      | string \| null                           | Optional description                                                                                                                                                           |
| emailAlias       | string                                   | Local-part of the resource's email address. Bookings can be created by inviting `<emailAlias>@resources.kalendme.com` as a guest on a calendar event.                          |
| timezone         | string                                   | IANA timezone used for interpreting `weekAvailability`                                                                                                                         |
| weekAvailability | [WeekAvailability](/#week-availability)  | Weekly availability windows (0=Sunday through 6=Saturday), in the resource's local time                                                                                        |
| acl              | [ResourceACL](/#resource-acl)            | Rule controlling who can book this resource                                                                                                                                    |
| metadata         | [ResourceMetadata](/#resource-metadata) \| null | Optional structured metadata about the resource                                                                                                                         |
| isEnabled        | boolean                                  | When `false`, new bookings are rejected with HTTP 409                                                                                                                          |
| isPubliclyListed | boolean                                  | When `true`, the resource appears on the organization's public booking page                                                                                                    |
| feedUrl          | string \| null                           | Public ICS feed URL. Includes the feed token — treat as a secret. Regenerate via the "Regenerate Feed Token" endpoint.                                                         |
| createdAt        | timestamp                                | The resource's creation timestamp                                                                                                                                              |
| updatedAt        | timestamp                                | The resource's last updated timestamp                                                                                                                                          |

## Resource Booking Resource

> An example resource booking model looks like this:

```json
{
  "booking": {
    "id": "rb_7c21e0",
    "resourceId": "r_9f8a1c",
    "organizerEmail": "alice@acme.com",
    "organizerName": "Alice Smith",
    "title": "Quarterly planning",
    "description": "Q2 strategy review",
    "startTimestamp": 1713441600000,
    "endTimestamp": 1713445200000,
    "startTimeUtc": "2026-04-18T14:00:00.000Z",
    "endTimeUtc": "2026-04-18T15:00:00.000Z",
    "status": "CONFIRMED",
    "bookingSource": "API",
    "createdAt": "2026-04-10T08:30:00.000Z",
    "updatedAt": "2026-04-10T08:30:00.000Z"
  }
}
```

| Parameter      | Type             | Description                                                                                                      |
| -------------- | ---------------- | ---------------------------------------------------------------------------------------------------------------- |
| id             | string           | The booking's id                                                                                                 |
| resourceId     | string           | The id of the resource this booking belongs to                                                                   |
| organizerEmail | string           | Email of the person booking the resource                                                                         |
| organizerName  | string           | Name of the person booking the resource                                                                          |
| title          | string           | Event title                                                                                                      |
| description    | string \| null   | Optional description                                                                                             |
| startTimestamp | int              | Start time in unix milliseconds                                                                                  |
| endTimestamp   | int              | End time in unix milliseconds                                                                                    |
| startTimeUtc   | timestamp        | Start time as an ISO 8601 UTC string                                                                             |
| endTimeUtc     | timestamp        | End time as an ISO 8601 UTC string                                                                               |
| status         | string           | `"CONFIRMED"` or `"CANCELLED"`                                                                                   |
| bookingSource  | string           | `"API"`, `"EMAIL"`, or `"PUBLIC_PAGE"` — how the booking was created                                             |
| createdAt      | timestamp        | The booking's creation timestamp                                                                                 |
| updatedAt      | timestamp        | The booking's last updated timestamp                                                                             |

## List Resources

```shell
curl "https://www.kalendme.com/api/v1/resources" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "resources": [
    {
      "id": "r_9f8a1c",
      "organizationId": "org_11223344",
      "name": "Boardroom A",
      "description": "10-seat boardroom on the 4th floor",
      "emailAlias": "boardroom-a",
      "timezone": "America/Sao_Paulo",
      "weekAvailability": { "0": [], "1": [{ "start": "09:00", "end": "18:00" }], "2": [{ "start": "09:00", "end": "18:00" }], "3": [{ "start": "09:00", "end": "18:00" }], "4": [{ "start": "09:00", "end": "18:00" }], "5": [{ "start": "09:00", "end": "18:00" }], "6": [] },
      "acl": { "mode": "org_members" },
      "metadata": { "capacity": 10 },
      "isEnabled": true,
      "isPubliclyListed": false,
      "feedUrl": "https://www.kalendme.com/api/resources/r_9f8a1c/feed/5f4d...abc1.ics",
      "createdAt": "2026-04-01T12:00:00.000Z",
      "updatedAt": "2026-04-10T08:30:00.000Z"
    }
  ]
}
```

Retrieves all resources in the authenticated API key's organization.

### HTTP Request

`GET https://www.kalendme.com/api/v1/resources`

## Create a Resource

```shell
curl "https://www.kalendme.com/api/v1/resources" \
  -X POST \
  -H "Authorization: Bearer abcdef123456" \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Boardroom A",
    "description": "10-seat boardroom on the 4th floor",
    "timezone": "America/Sao_Paulo",
    "weekAvailability": {
      "0": [],
      "1": [{ "start": "09:00", "end": "18:00" }],
      "2": [{ "start": "09:00", "end": "18:00" }],
      "3": [{ "start": "09:00", "end": "18:00" }],
      "4": [{ "start": "09:00", "end": "18:00" }],
      "5": [{ "start": "09:00", "end": "18:00" }],
      "6": []
    },
    "acl": { "mode": "org_members" },
    "metadata": { "capacity": 10, "floor": 4, "building": "HQ" },
    "emailAlias": "boardroom-a"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "resource": {
    "id": "r_9f8a1c",
    "organizationId": "org_11223344",
    "name": "Boardroom A",
    "description": "10-seat boardroom on the 4th floor",
    "emailAlias": "boardroom-a",
    "timezone": "America/Sao_Paulo",
    "weekAvailability": { "0": [], "1": [{ "start": "09:00", "end": "18:00" }], "2": [{ "start": "09:00", "end": "18:00" }], "3": [{ "start": "09:00", "end": "18:00" }], "4": [{ "start": "09:00", "end": "18:00" }], "5": [{ "start": "09:00", "end": "18:00" }], "6": [] },
    "acl": { "mode": "org_members" },
    "metadata": { "capacity": 10, "floor": 4, "building": "HQ" },
    "isEnabled": true,
    "isPubliclyListed": false,
    "feedUrl": "https://www.kalendme.com/api/resources/r_9f8a1c/feed/5f4d...abc1.ics",
    "createdAt": "2026-04-01T12:00:00.000Z",
    "updatedAt": "2026-04-01T12:00:00.000Z"
  }
}
```

Creates a new resource in the organization. Each resource consumes a seat against the organization's plan, so creation is rejected with HTTP 403 (error 1065) if the seat limit is reached.

### HTTP Request

`POST https://www.kalendme.com/api/v1/resources`

### Body Parameters

| Parameter        | Type                                           | Required | Description                                                                                                                                                  |
| ---------------- | ---------------------------------------------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| name             | string                                         | Required | Resource name (1–50 characters).                                                                                                                             |
| description      | string                                         | Optional | Free-form description.                                                                                                                                       |
| timezone         | string                                         | Required | IANA timezone used for interpreting `weekAvailability`.                                                                                                      |
| weekAvailability | [WeekAvailability](/#week-availability)        | Required | Weekly booking windows, keyed `0` (Sunday) through `6` (Saturday), each a list of `{ start, end }` objects in 24-hour `HH:MM`. Use `"24:00"` for end-of-day. |
| acl              | [ResourceACL](/#resource-acl)                  | Optional | Defaults to `{ "mode": "public" }` if omitted.                                                                                                               |
| metadata         | [ResourceMetadata](/#resource-metadata)        | Optional | Structured metadata (capacity, floor, building, equipment).                                                                                                  |
| emailAlias       | string                                         | Optional | Local-part of the booking email (`[a-z0-9-]{1,36}`). If omitted, a random alias is generated.                                                                |

<aside class="notice">If <code>emailAlias</code> collides with an existing resource in any organization, the request fails with HTTP 409.</aside>

## Get a Resource

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "resource": {
    "id": "r_9f8a1c",
    "organizationId": "org_11223344",
    "name": "Boardroom A",
    "description": "10-seat boardroom on the 4th floor",
    "emailAlias": "boardroom-a",
    "timezone": "America/Sao_Paulo",
    "weekAvailability": { "1": [{ "start": "09:00", "end": "18:00" }] },
    "acl": { "mode": "org_members" },
    "metadata": { "capacity": 10 },
    "isEnabled": true,
    "isPubliclyListed": false,
    "feedUrl": "https://www.kalendme.com/api/resources/r_9f8a1c/feed/5f4d...abc1.ics",
    "createdAt": "2026-04-01T12:00:00.000Z",
    "updatedAt": "2026-04-10T08:30:00.000Z"
  }
}
```

Retrieves a single resource owned by the authenticated organization.

### HTTP Request

`GET https://www.kalendme.com/api/v1/resources/<resourceId>`

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |

## Update a Resource

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c" \
  -X PATCH \
  -H "Authorization: Bearer abcdef123456" \
  -H "Content-Type: application/json" \
  -d '{
    "description": "10-seat boardroom with video conferencing",
    "acl": { "mode": "domain", "domains": ["acme.com"] },
    "isEnabled": true
  }'
```

> The above command returns JSON structured like this:

```json
{
  "resource": {
    "id": "r_9f8a1c",
    "organizationId": "org_11223344",
    "name": "Boardroom A",
    "description": "10-seat boardroom with video conferencing",
    "emailAlias": "boardroom-a",
    "timezone": "America/Sao_Paulo",
    "weekAvailability": { "1": [{ "start": "09:00", "end": "18:00" }] },
    "acl": { "mode": "domain", "domains": ["acme.com"] },
    "metadata": { "capacity": 10 },
    "isEnabled": true,
    "isPubliclyListed": false,
    "feedUrl": "https://www.kalendme.com/api/resources/r_9f8a1c/feed/5f4d...abc1.ics",
    "createdAt": "2026-04-01T12:00:00.000Z",
    "updatedAt": "2026-04-11T09:15:00.000Z"
  }
}
```

Partially updates a resource. Only fields present in the body are changed.

### HTTP Request

`PATCH https://www.kalendme.com/api/v1/resources/<resourceId>`

### Body Parameters

| Parameter        | Type                                    | Required | Description                                                                                        |
| ---------------- | --------------------------------------- | -------- | -------------------------------------------------------------------------------------------------- |
| name             | string                                  | Optional | Resource name (1–50 characters).                                                                   |
| description      | string                                  | Optional | Free-form description. Pass an empty string to clear.                                              |
| timezone         | string                                  | Optional | IANA timezone.                                                                                     |
| weekAvailability | [WeekAvailability](/#week-availability) | Optional | Replaces the entire weekly availability.                                                           |
| acl              | [ResourceACL](/#resource-acl)           | Optional | Replaces the ACL.                                                                                  |
| metadata         | [ResourceMetadata](/#resource-metadata) | Optional | Replaces the metadata. Pass `null` to clear.                                                       |
| isEnabled        | boolean                                 | Optional | Toggle whether the resource accepts new bookings.                                                  |

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |

## Delete a Resource

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c" \
  -X DELETE \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "success": true
}
```

Soft-deletes the resource. Its `emailAlias` is released and the resource disappears from list endpoints. Existing bookings remain in the database but cannot be re-fetched through v1.

### HTTP Request

`DELETE https://www.kalendme.com/api/v1/resources/<resourceId>`

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |

## Regenerate Feed Token

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c" \
  -X POST \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "resource": {
    "id": "r_9f8a1c",
    "feedUrl": "https://www.kalendme.com/api/resources/r_9f8a1c/feed/9a1c...ef02.ics",
    "updatedAt": "2026-04-11T10:00:00.000Z"
  }
}
```

Rotates the resource's ICS `feedToken`, invalidating the previous `feedUrl`. The full resource (with the new `feedUrl`) is returned.

### HTTP Request

`POST https://www.kalendme.com/api/v1/resources/<resourceId>`

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |

<aside class="warning">Any subscribers to the old feed URL will stop receiving updates.</aside>

## Get Availability for a Resource

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c/availability?startTimestamp=1713398400000&endTimestamp=1713657600000&timeZone=America/Sao_Paulo" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "resourceId": "r_9f8a1c",
  "timezone": "America/Sao_Paulo",
  "availableSlots": [
    { "start": "2026-04-18T09:00:00.000-03:00", "end": "2026-04-18T11:00:00.000-03:00" },
    { "start": "2026-04-18T12:00:00.000-03:00", "end": "2026-04-18T18:00:00.000-03:00" }
  ]
}
```

Computes the free slots for a resource within a time range by subtracting existing bookings from the resource's weekly availability. Slots are returned in the requested `timeZone` (falls back to the resource's own timezone if missing or invalid).

### HTTP Request

`GET https://www.kalendme.com/api/v1/resources/<resourceId>/availability`

### Query Parameters

| Parameter      | Type   | Required | Description                                                                    |
| -------------- | ------ | -------- | ------------------------------------------------------------------------------ |
| startTimestamp | int    | Required | Start of the window in unix milliseconds.                                      |
| endTimestamp   | int    | Required | End of the window in unix milliseconds. Must be after `startTimestamp`.        |
| timeZone       | string | Optional | IANA timezone to emit ISO strings in. Defaults to the resource's `timezone`.   |

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |

## List Resource Bookings

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c/bookings?startTimestamp=1713398400000&endTimestamp=1714003200000" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "bookings": [
    {
      "id": "rb_7c21e0",
      "resourceId": "r_9f8a1c",
      "organizerEmail": "alice@acme.com",
      "organizerName": "Alice Smith",
      "title": "Quarterly planning",
      "description": null,
      "startTimestamp": 1713441600000,
      "endTimestamp": 1713445200000,
      "startTimeUtc": "2026-04-18T14:00:00.000Z",
      "endTimeUtc": "2026-04-18T15:00:00.000Z",
      "status": "CONFIRMED",
      "bookingSource": "API",
      "createdAt": "2026-04-10T08:30:00.000Z",
      "updatedAt": "2026-04-10T08:30:00.000Z"
    }
  ]
}
```

Lists bookings for a resource. When both timestamps are provided, only bookings overlapping the `[startTimestamp, endTimestamp]` range are returned. If neither is provided, all bookings for the resource are returned.

### HTTP Request

`GET https://www.kalendme.com/api/v1/resources/<resourceId>/bookings`

### Query Parameters

| Parameter      | Type | Required | Description                                |
| -------------- | ---- | -------- | ------------------------------------------ |
| startTimestamp | int  | Optional | Start of the window in unix milliseconds.  |
| endTimestamp   | int  | Optional | End of the window in unix milliseconds.    |

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |

## Create a Resource Booking

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c/bookings" \
  -X POST \
  -H "Authorization: Bearer abcdef123456" \
  -H "Content-Type: application/json" \
  -d '{
    "startTimestamp": 1713441600000,
    "endTimestamp": 1713445200000,
    "organizerEmail": "alice@acme.com",
    "organizerName": "Alice Smith",
    "title": "Quarterly planning",
    "description": "Q2 strategy review"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "booking": {
    "id": "rb_7c21e0",
    "resourceId": "r_9f8a1c",
    "organizerEmail": "alice@acme.com",
    "organizerName": "Alice Smith",
    "title": "Quarterly planning",
    "description": "Q2 strategy review",
    "startTimestamp": 1713441600000,
    "endTimestamp": 1713445200000,
    "startTimeUtc": "2026-04-18T14:00:00.000Z",
    "endTimeUtc": "2026-04-18T15:00:00.000Z",
    "status": "CONFIRMED",
    "bookingSource": "API",
    "createdAt": "2026-04-10T08:30:00.000Z",
    "updatedAt": "2026-04-10T08:30:00.000Z"
  }
}
```

Books the resource for the given time window. The endpoint enforces:

* The resource is enabled (`isEnabled: true`) — otherwise `1070`.
* The `organizerEmail` is authorized by the resource's [ACL](/#resource-acl) — otherwise `1071`.
* The window fits within the resource's `weekAvailability` — otherwise `1069`.
* The window does not collide with an existing booking — otherwise `1068`. Conflict detection uses a serialized transaction to make concurrent bookings safe.

### HTTP Request

`POST https://www.kalendme.com/api/v1/resources/<resourceId>/bookings`

### Body Parameters

| Parameter      | Type   | Required | Description                                                   |
| -------------- | ------ | -------- | ------------------------------------------------------------- |
| startTimestamp | int    | Required | Booking start in unix milliseconds.                           |
| endTimestamp   | int    | Required | Booking end in unix milliseconds. Must be after start.        |
| organizerEmail | string | Required | Email of the person booking the resource. Validated by ACL.   |
| organizerName  | string | Required | Name of the person booking the resource.                      |
| title          | string | Required | Event title.                                                  |
| description    | string | Optional | Free-form description.                                        |

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |

## Get a Resource Booking

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c/bookings/rb_7c21e0" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "booking": {
    "id": "rb_7c21e0",
    "resourceId": "r_9f8a1c",
    "organizerEmail": "alice@acme.com",
    "organizerName": "Alice Smith",
    "title": "Quarterly planning",
    "description": "Q2 strategy review",
    "startTimestamp": 1713441600000,
    "endTimestamp": 1713445200000,
    "startTimeUtc": "2026-04-18T14:00:00.000Z",
    "endTimeUtc": "2026-04-18T15:00:00.000Z",
    "status": "CONFIRMED",
    "bookingSource": "API",
    "createdAt": "2026-04-10T08:30:00.000Z",
    "updatedAt": "2026-04-10T08:30:00.000Z"
  }
}
```

### HTTP Request

`GET https://www.kalendme.com/api/v1/resources/<resourceId>/bookings/<bookingId>`

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |
| bookingId  | string | Required | The id of the booking.       |

## Cancel a Resource Booking

```shell
curl "https://www.kalendme.com/api/v1/resources/r_9f8a1c/bookings/rb_7c21e0" \
  -X DELETE \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "booking": {
    "id": "rb_7c21e0",
    "resourceId": "r_9f8a1c",
    "organizerEmail": "alice@acme.com",
    "organizerName": "Alice Smith",
    "title": "Quarterly planning",
    "description": "Q2 strategy review",
    "startTimestamp": 1713441600000,
    "endTimestamp": 1713445200000,
    "startTimeUtc": "2026-04-18T14:00:00.000Z",
    "endTimeUtc": "2026-04-18T15:00:00.000Z",
    "status": "CANCELLED",
    "bookingSource": "API",
    "createdAt": "2026-04-10T08:30:00.000Z",
    "updatedAt": "2026-04-11T09:45:00.000Z"
  }
}
```

Cancels a booking by setting `status` to `CANCELLED`. Cancelled bookings no longer block conflicting windows.

### HTTP Request

`DELETE https://www.kalendme.com/api/v1/resources/<resourceId>/bookings/<bookingId>`

### URL Parameters

| Parameter  | Type   | Required | Description                  |
| ---------- | ------ | -------- | ---------------------------- |
| resourceId | string | Required | The id of the resource.      |
| bookingId  | string | Required | The id of the booking.       |

## Find Available Resources

```shell
curl "https://www.kalendme.com/api/v1/organizations/resources/available?startTimestamp=1713441600000&endTimestamp=1713445200000" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "resources": [
    {
      "id": "r_9f8a1c",
      "organizationId": "org_11223344",
      "name": "Boardroom A",
      "emailAlias": "boardroom-a",
      "timezone": "America/Sao_Paulo",
      "weekAvailability": { "1": [{ "start": "09:00", "end": "18:00" }] },
      "acl": { "mode": "org_members" },
      "metadata": { "capacity": 10 },
      "isEnabled": true,
      "isPubliclyListed": false,
      "feedUrl": "https://www.kalendme.com/api/resources/r_9f8a1c/feed/5f4d...abc1.ics",
      "createdAt": "2026-04-01T12:00:00.000Z",
      "updatedAt": "2026-04-10T08:30:00.000Z"
    }
  ]
}
```

Returns every enabled resource in the organization that has no booking conflicts **and** whose `weekAvailability` fully covers the requested window. Useful for "find a room" UIs.

### HTTP Request

`GET https://www.kalendme.com/api/v1/organizations/resources/available`

### Query Parameters

| Parameter      | Type | Required | Description                                                             |
| -------------- | ---- | -------- | ----------------------------------------------------------------------- |
| startTimestamp | int  | Required | Start of the desired window in unix milliseconds.                       |
| endTimestamp   | int  | Required | End of the desired window in unix milliseconds. Must be after start.    |

## Public Organization Resources

```shell
curl "https://www.kalendme.com/api/public/organizations/acme/resources?startTimestamp=1713398400000&endTimestamp=1713657600000&token=5f4d...abc1"
```

> The above command returns JSON structured like this:

```json
{
  "organization": {
    "name": "Acme Inc.",
    "slug": "acme"
  },
  "resources": [
    {
      "id": "r_9f8a1c",
      "name": "Boardroom A",
      "description": "10-seat boardroom on the 4th floor",
      "emailAlias": "boardroom-a",
      "timezone": "America/Sao_Paulo",
      "weekAvailability": { "1": [{ "start": "09:00", "end": "18:00" }] },
      "metadata": { "capacity": 10 }
    }
  ],
  "bookings": [
    {
      "id": "rb_7c21e0",
      "resourceId": "r_9f8a1c",
      "startTimeUtc": "2026-04-18T14:00:00.000Z",
      "endTimeUtc": "2026-04-18T15:00:00.000Z",
      "title": "Quarterly planning",
      "organizerName": "Alice Smith",
      "organizerEmail": "alice@acme.com"
    }
  ]
}
```

Public, unauthenticated endpoint that backs an organization's public booking page. Returns every resource the organization has marked `isPubliclyListed: true` and any bookings that overlap the requested window (up to 7 days).

When the organization has enabled a `publicPageToken`, the `token` query parameter must match exactly or the request fails with error `1076`. If the organization's `publicShowBookingDetails` setting is `false`, the `title`, `organizerName`, and `organizerEmail` fields are omitted from each booking.

<aside class="notice">This endpoint is "Public" — no <code>Authorization</code> header is required, but public-page access must be enabled on the organization and the request must match the slug/token configured by the admin.</aside>

### HTTP Request

`GET https://www.kalendme.com/api/public/organizations/<slug>/resources`

### Query Parameters

| Parameter      | Type   | Required    | Description                                                                                        |
| -------------- | ------ | ----------- | -------------------------------------------------------------------------------------------------- |
| startTimestamp | int    | Required    | Start of the desired window in unix milliseconds.                                                  |
| endTimestamp   | int    | Required    | End of the desired window in unix milliseconds. Must be after start and within 7 days of it.       |
| token          | string | Conditional | Required if the organization has configured a `publicPageToken`; omit otherwise.                   |

### URL Parameters

| Parameter | Type   | Required | Description                        |
| --------- | ------ | -------- | ---------------------------------- |
| slug      | string | Required | The organization's public slug.    |

## Resource ICS Feed

```shell
curl "https://www.kalendme.com/api/resources/r_9f8a1c/feed/5f4d...abc1.ics"
```

> The above command returns an ICS calendar document:

```
BEGIN:VCALENDAR
VERSION:2.0
PRODID:-//KalendMe//EN
CALSCALE:GREGORIAN
METHOD:PUBLISH
X-WR-CALNAME:Boardroom A
BEGIN:VEVENT
UID:booking-rb_7c21e0@kalendme.com
DTSTAMP:20260410T083000Z
DTSTART:20260418T140000Z
DTEND:20260418T150000Z
SUMMARY:Quarterly planning
DESCRIPTION:Alice Smith (alice@acme.com)
STATUS:CONFIRMED
TRANSP:OPAQUE
END:VEVENT
END:VCALENDAR
```

Public, unauthenticated ICS feed subscribed to from Google Calendar, Outlook, Apple Calendar, etc. Authentication is by bearer-equivalent secret: the `feedToken` in the URL path. Returns every booking from 30 days in the past through 90 days in the future.

<aside class="warning">Anyone with the feed URL can read bookings. Rotate via <a href="#regenerate-feed-token">Regenerate Feed Token</a> to invalidate a leaked URL.</aside>

### HTTP Request

`GET https://www.kalendme.com/api/resources/<resourceId>/feed/<feedToken>.ics`

The `.ics` suffix is optional but recommended — most calendar clients require it to recognize the response.

### URL Parameters

| Parameter  | Type   | Required | Description                                                      |
| ---------- | ------ | -------- | ---------------------------------------------------------------- |
| resourceId | string | Required | The id of the resource.                                          |
| feedToken  | string | Required | The secret feed token generated on resource creation/rotation.   |

### Response

`Content-Type: text/calendar; charset=utf-8` with an RFC 5545 iCalendar document. Cancelled bookings are not included.
