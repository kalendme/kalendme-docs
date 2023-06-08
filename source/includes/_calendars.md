# Calendars

## Calendar Resource

> An example calendar model looks like this:

```json
{
  "calendar": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "accountId": "123213232",
    "name": "My Calendar",
    "isInput": true,
    "isOutput": true,
    "isReadOnly": true
  }
}
```

Calendars belong to user [calendar accounts](#calendar-accounts). They are the calendars that the user has access to. They can be input, output or both. They can also be read-only.

| Parameter  | Type      | Description                                                   |
| ---------- | --------- | ------------------------------------------------------------- |
| id         | string    | The resource's id                                             |
| createdAt  | timestamp | The resource's creation timesstamp                            |
| updatedAt  | timestamp | The resource's last updated timestamp                         |
| userId     | string    | The ID of the user that this calendar belongs to.             |
| accountId  | string    | The ID of the calendar account that this calendar belongs to. |
| name       | string    | The name of the calendar                                      |
| isInput    | boolean   | Whether this calendar is an input calendar                    |
| isOutput   | boolean   | Whether this calendar is an output calendar                   |
| isReadOnly | boolean   | Whether this calendar is read-only                            |

## Connect a Calendar

Calendars are created after connecting a Calendar account automatically since they come from the account's provider. You don't need to create them manually.

## Get a user's Calendars

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendars" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "calendars": [
    {
      "id": "123213232",
      "userId": "123123123",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "accountId": "123213232",
      "name": "My Calendar",
      "isInput": true,
      "isOutput": true,
      "isReadOnly": false
    },
    {
      "id": "123213232",
      "userId": "123123123",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "accountId": "123213232",
      "name": "Holidays in Brazil",
      "isInput": true,
      "isOutput": false,
      "isReadOnly": true
    }
  ]
}
```

This endpoint retrieves all configured calendars for a user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/calendars`

### URL Parameters

| Parameter | Type   | Required | Description                                |
| --------- | ------ | -------- | ------------------------------------------ |
| userId    | string | Required | The ID of the user's calendars to retrieve |

## Get a Specific Calendar

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendars/123213232" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "calendar": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "accountId": "123213232",
    "name": "Holidays in Brazil",
    "isInput": true,
    "isOutput": false,
    "isReadOnly": true
  }
}
```

This endpoint retrieves a specific calendar for a user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/calendars/<calendarId>`

### URL Parameters

| Parameter  | Type   | Required | Description                                        |
| ---------- | ------ | -------- | -------------------------------------------------- |
| userId     | string | Required | The ID of the user's calendar accounts to retrieve |
| calendarId | string | Required | The ID of the calendar to retrieve                 |

## Update a Calendar

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendars/123213232" \
  -X PATCH \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "calendar": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "accountId": "123213232",
    "name": "Holidays in Brazil",
    "isInput": true,
    "isOutput": false,
    "isReadOnly": true
  }
}
```

This endpoint updates a calendar for a specific user. Use it to configure the calendar's input, and output properties. This way a user can control which calendars they want to use as input calendars when calculating their availability, and which calendar they want to use as output when sharing their availability so that events can be created there.

### HTTP Request

`PATCH https://www.kalendme.com/api/v1/users/<userId>/calendars/<calendarId>`

### URL Parameters

| Parameter  | Type   | Required | Description                                        |
| ---------- | ------ | -------- | -------------------------------------------------- |
| userId     | string | Required | The ID of the user's calendar accounts to retrieve |
| calendarId | string | Required | The ID of the calendar to retrieve                 |

### Body Parameters

| Parameter | Type    | Required | Description                                 |
| --------- | ------- | -------- | ------------------------------------------- |
| isInput   | boolean | Optional | Whether this calendar is an input calendar  |
| isOutput  | boolean | Optional | Whether this calendar is an output calendar |
