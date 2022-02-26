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
    1234567890000
  ],
  "potentialAvailableDateTimes": [
    // When passing the optional flag `includePotentialAvailableDateTimes`
    1234567890000, // Timestamps in epoch milliseconds
    1234567890000,
    1234567890000
  ]
}
```

This endpoint returns a user's general availability for the requrested month, year and duration in minutes.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/availability`

### URL Parameters

| Parameter                          | Type    | Required | Description                                                                                                                  |
| ---------------------------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| userId                             | string  | Required | The id of the user.                                                                                                          |
| startTimestamp                     | int     | Required | The start timestamp in epoch milliseconds to start the query for.                                                            |
| endTimestamp                       | int     | Required | The end timestamp in epoch milliseconds to start the query for. It cannot span for more than a month.                        |
| durationMinutes                    | int     | Required | The duration of the session you want to schedule this user.                                                                  |
| timeZone                           | int     | Required | The time zone you want to get the availability for.                                                                          |
| year                               | int     | Optional | Instead of querying a specific startTimestamp and endTimestamp you can pass in a year and month.                             |
| month                              | int     | Optional | Instead of querying a specific startTimestamp and endTimestamp you can pass in a year and month.                             |
| includePotentialAvailableDateTimes | boolean | Optional | Output an optional array of what would be all the potential available date times for a user if they did not have any events. |

## Get a User's Link Availability

```shell
curl "https://www.kalendme.com/api/v1/users/123123/links/654654/availability?month=10&year=2021" \
  -X GET
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "availableDateTimes": [
    1234567890000, // Timestamps in epoch milliseconds
    1234567890000,
    1234567890000
  ],
  "potentialAvailableDateTimes": [
    // When passing the optional flag `includePotentialAvailableDateTimes`
    1234567890000, // Timestamps in epoch milliseconds
    1234567890000,
    1234567890000
  ]
}
```

This endpoint returns a user's specific link's availability for the requested month and year.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/links/<linkId>/availability`

### URL Parameters

| Parameter                          | Type    | Required | Description                                                                                                                  |
| ---------------------------------- | ------- | -------- | ---------------------------------------------------------------------------------------------------------------------------- |
| userId                             | string  | Required | The id of the user.                                                                                                          |
| startTimestamp                     | int     | Required | The start timestamp in epoch milliseconds to start the query for.                                                            |
| endTimestamp                       | int     | Required | The end timestamp in epoch milliseconds to start the query for. It cannot span for more than a month.                        |
| durationMinutes                    | int     | Required | The duration of the session you want to schedule this user.                                                                  |
| timeZone                           | int     | Required | The time zone you want to get the availability for.                                                                          |
| year                               | int     | Optional | Instead of querying a specific startTimestamp and endTimestamp you can pass in a year and month.                             |
| month                              | int     | Optional | Instead of querying a specific startTimestamp and endTimestamp you can pass in a year and month.                             |
| includePotentialAvailableDateTimes | boolean | Optional | Output an optional array of what would be all the potential available date times for a user if they did not have any events. |
