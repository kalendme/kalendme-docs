# User Availability

General booking endpoints to consult user's availaibilities.

## Get a User's Availability

```shell
curl "https://www.kalendme.com/api/v1/users/123123/availability?month=10&year=2021&durationMinutes=30" \
  -X GET
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "availableDateTimes": [
    1234567890000, // Timestamps in epoch milliseconds  
    1234567890000,
    1234567890000,
  ]
}
```

This endpoint returns a user's general availability for the requrested month, year and duration in minutes.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/availability`

### URL Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user.
year | int | Required | The year you want to query the availability for.
month | int | Required | The month you want to query the availability for.
durationMinutes | int | Required | The duration of the session you want to schedule this user.

## Get a User's Event Type Availability

```shell
curl "https://www.kalendme.com/api/v1/users/123123/event-types/654654/availability?month=10&year=2021" \
  -X GET
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "availableDateTimes": [
    1234567890000, // Timestamps in epoch milliseconds  
    1234567890000,
    1234567890000,
  ]
}
```

This endpoint returns a user's specific event type's availability for the requrested month and year.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/event-types/<eventTypeId>/availability`

### URL Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user.
eventTypeId | string | Required | The id of the user event type.
year | int | Required | The year you want to query the availability for.
month | int | Required | The month you want to query the availability for.