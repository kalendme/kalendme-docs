# Calendar Accounts

## Calendar Account Resource

> An example calendar account model looks like this:

```json
{
  "calendarAccount": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "type": "KALENDME",
    "nickname": "My Calendar"
  }
}
```

Calendar accounts belong to users, they are unique calendars that can contain several events and can be used to calculate a user's availability. Calendar accounts are needed if you want that user to use a KalendMe calendar without needing to connect their Google/Microsoft accounts. Calendar Accounts are formed by the following fields.

Parameter | Type | Description
--------- | ---- | -----------
id | string | The resource's id
createdAt | timestamp | The resource's creation timesstamp
updatedAt | timestamp | The resource's last updated timestamp
userId | string | The ID of the user that this calendar belongs to.
type | string | The type of the calendar account.
nickname | string | A description of the calendar account.

## Create a Calendar Account

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendar-accounts?linkCalendarAccountAsInput=true" \
  -X POST
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "nickname": "My Calendar"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "calendarAccount": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "type": "KALENDME",
    "nickname": "My Calendar"
  }
}
```

This endpoint creates a new calendar account for a user.

### HTTP Request

`POST https://www.kalendme.com/api/v1/users/<userId>/calendar-accounts`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
nickname | string | Required | A description of this user's calendar account.

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this calendar account will belong to.
linkCalendarAccountAsInput | boolean | Optional | `Default: false` This links the newly created calendar account to the user’s general availability. In other words, when you query for available date times, these calendar events will affect and block calendar times if there’s events present.
linkCalendarAccountAsOutput | boolean | Optional | `Default: false` This links the newly created calendar account to be the user’s main calendar for creating events. All scheduled events will be created on this calendar account.


## Get a user's Calendar Accounts

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendar-accounts" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "calendarAccounts": [
    {
      "id": "123213232",
      "userId": "123123123",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "type": "KALENDME",
      "nickname": "My Personal Calendar"
    },
    {
      "id": "123213232",
      "userId": "123123123",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "type": "KALENDME",
      "nickname": "My Work Calendar"
    }
  ]
}

```

This endpoint retrieves all calendar accounts for a user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/calendar-accounts`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The ID of the user's calendar accounts to retrieve

## Get a Specific Calendar Account

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendar-accounts/123213232" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "calendarAccount": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "type": "KALENDME",
    "nickname": "My Calendar"
  }
}
```

This endpoint retrieves a specific calendar account for a user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/calendar-accounts/<calendarAccountId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The ID of the user's calendar accounts to retrieve
calendarAccountId | string | Required | The ID of the calendar account to retrieve
