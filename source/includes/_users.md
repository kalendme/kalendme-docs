# Users

## User Resource

> An example user model looks like this:

```json
{
  "user": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "name": "Jim Halpert",
    "email": "jim@dundermifflin.com",
    "urlString": "jim-123456",
    "timeZone": "America/New_York",
    "status": "ACTIVE",
    "weekAvailability": {
      "0": [ { "start": "00:00", "end": "00:00" } ],
      "1": [ { "start": "08:00", "end": "18:00" } ],
      "2": [ { "start": "08:00", "end": "18:00" } ],
      "3": [ { "start": "08:00", "end": "18:00" } ],
      "4": [ { "start": "08:00", "end": "18:00" } ],
      "5": [ { "start": "08:00", "end": "18:00" } ],
      "6": [ { "start": "08:00", "end": "18:00" } ],
    }
  }
}
```

Users are all users under your organization. Users are formed by the following fields.

Parameter | Type | Description
--------- | ---- | -----------
id | string | The resource's id
createdAt | timestamp | The resource's creation timesstamp
updatedAt | timestamp | The resource's last updated timestamp
email | string | The email of the user. Must be a valid email address.
name | string | The name of the user
language | string | The language of the user. We currently support only `en` (english), `es` (spanish) and `pt` (portuguese).
urlString | string | A globally unique URL for the user. Used for their bookings page such as kalendme.com/`john-123`. If none is specified, a random one will be generated on user creation.
timeZone | string | The time zone of the user. Must be a valid [IANA time zone](https://en.wikipedia.org/wiki/Tz_database) name such as `America/New_York`.
status | string | The status of the user. Can only be: `ACTIVE` or `INVITED`. Note that invited users do not affect your bill until they accept to join your organization.
weekAvailability | [WeekAvailability](/#week-availability) | An object containing the user's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times.

## Create a User

```shell
curl "https://www.kalendme.com/api/v1/users?sendInvitationEmail=true" \
  -X POST
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "name": "Jim Halpert",
    "email": "jim@dundermifflin.com",
    "language": "en",
    "timeZone": "America/New_York"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "user": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "name": "Jim Halpert",
    "email": "jim@dundermifflin.com",
    "urlString": "jim-123456",
    "timeZone": "America/New_York",
    "weekAvailability": {
      "0": [ { "start": "00:00", "end": "00:00" } ],
      "1": [ { "start": "08:00", "end": "18:00" } ],
      "2": [ { "start": "08:00", "end": "18:00" } ],
      "3": [ { "start": "08:00", "end": "18:00" } ],
      "4": [ { "start": "08:00", "end": "18:00" } ],
      "5": [ { "start": "08:00", "end": "18:00" } ],
      "6": [ { "start": "08:00", "end": "18:00" } ],
    }
  }
}
```

This endpoint creates a new user under your organization.

### HTTP Request

`POST https://www.kalendme.com/api/v1/users`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
email | string | Required | The email of the user. Must be a valid email address.
name | string | Optional | The name of the user
language | string | Required | The language of the user. We currently support only `en` (english), `es` (spanish) and `pt` (portuguese).
urlString | string | Optional | A globally unique URL for the user. Used for their bookings page such as kalendme.com/`john-123`. If none is specified, a random one will be generated on user creation.
timeZone | string | Required | The time zone of the user. Must be a valid [IANA time zone](https://en.wikipedia.org/wiki/Tz_database) name such as `America/New_York`.
weekAvailability | [WeekAvailability](/#week-availability) | Optional | An object containing the user's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times. 

### URL Parameters

Parameter | Type | Description
--------- | ---- | -----------
sendInvitationEmail | boolean | Send the newly created user an invitation email on behalf of your organization to join your organization and configure their account.

## Get All Users

```shell
curl "https://www.kalendme.com/api/v1/users" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "users": [
    {
      "id": "123213233",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "name": "Michael Scott",
      "email": "michael@dundermifflin.com",
      "urlString": "michael-scott",
      "timeZone": "America/New_York",
      "weekAvailability": {
        "0": [ { "start": "00:00", "end": "00:00" } ],
        "1": [ { "start": "08:00", "end": "18:00" } ],
        "2": [ { "start": "08:00", "end": "18:00" } ],
        "3": [ { "start": "08:00", "end": "18:00" } ],
        "4": [ { "start": "08:00", "end": "18:00" } ],
        "5": [ { "start": "08:00", "end": "18:00" } ],
        "6": [ { "start": "08:00", "end": "18:00" } ],
      }
    },
    {
      "id": "123213232",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "name": "Pam Beesly",
      "email": "pam@dundermifflin.com",
      "urlString": "pam-beesly",
      "timeZone": "America/New_York",
      "weekAvailability": {
        "0": [ { "start": "00:00", "end": "00:00" } ],
        "1": [ { "start": "08:00", "end": "18:00" } ],
        "2": [ { "start": "08:00", "end": "18:00" } ],
        "3": [ { "start": "08:00", "end": "18:00" } ],
        "4": [ { "start": "08:00", "end": "18:00" } ],
        "5": [ { "start": "08:00", "end": "18:00" } ],
        "6": [ { "start": "08:00", "end": "18:00" } ],
      }
    },
  ]
}

```

This endpoint retrieves all users.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users`

## Get a Specific User

```shell
curl "https://www.kalendme.com/api/v1/users/123213232" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "user": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "name": "Pam Beesly",
    "email": "pam@dundermifflin.com",
    "urlString": "pam-beesly",
    "timeZone": "America/New_York",
    "weekAvailability": {
      "0": [ { "start": "00:00", "end": "00:00" } ],
      "1": [ { "start": "08:00", "end": "18:00" } ],
      "2": [ { "start": "08:00", "end": "18:00" } ],
      "3": [ { "start": "08:00", "end": "18:00" } ],
      "4": [ { "start": "08:00", "end": "18:00" } ],
      "5": [ { "start": "08:00", "end": "18:00" } ],
      "6": [ { "start": "08:00", "end": "18:00" } ],
    }
  }
}
```

This endpoint retrieves a specific user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The ID of the user to retrieve

## Update a User

```shell
curl "https://www.kalendme.com/api/v1/users" \
  -X PATCH
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "timeZone": "America/Los_Angeles"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "user": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "name": "Jim Halpert",
    "email": "jim@dundermifflin.com",
    "urlString": "jim-123456",
    "timeZone": "America/Los_Angeles",
    "weekAvailability": {
      "0": [ { "start": "00:00", "end": "00:00" } ],
      "1": [ { "start": "08:00", "end": "18:00" } ],
      "2": [ { "start": "08:00", "end": "18:00" } ],
      "3": [ { "start": "08:00", "end": "18:00" } ],
      "4": [ { "start": "08:00", "end": "18:00" } ],
      "5": [ { "start": "08:00", "end": "18:00" } ],
      "6": [ { "start": "08:00", "end": "18:00" } ],
    }
  }
}
```

This endpoint updates a user under your organization.

### HTTP Request

`PATCH https://www.kalendme.com/api/v1/users/<userId>`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
name | string | Optional | The name of the user
language | string | Optional | The language of the user. We currently support only `en` (english), `es` (spanish) and `pt` (portuguese).
urlString | string | Optional | A globally unique URL for the user. Used for their bookings page such as kalendme.com/`john-123`.
timeZone | string | Optional | The time zone of the user. Must be a valid [IANA time zone](https://en.wikipedia.org/wiki/Tz_database) name such as `America/New_York`.
weekAvailability | [WeekAvailability](/#week-availability) | Optional | An object containing the user's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times.

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The ID of the user to update

## Delete a Specific User

```shell
curl "https://www.kalendme.com/api/v1/users/123213232" \
  -X DELETE
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "user": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "name": "Pam Beesly",
    "email": "pam@dundermifflin.com",
    "urlString": "pam-beesly",
    "timeZone": "America/New_York",
    "weekAvailability": {
      "0": [ { "start": "00:00", "end": "00:00" } ],
      "1": [ { "start": "08:00", "end": "18:00" } ],
      "2": [ { "start": "08:00", "end": "18:00" } ],
      "3": [ { "start": "08:00", "end": "18:00" } ],
      "4": [ { "start": "08:00", "end": "18:00" } ],
      "5": [ { "start": "08:00", "end": "18:00" } ],
      "6": [ { "start": "08:00", "end": "18:00" } ],
    }
  }
}
```

This endpoint deletes a specific user from your organization.

### HTTP Request

`DELETE https://www.kalendme.com/api/v1/users/<userId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The ID of the user to delete