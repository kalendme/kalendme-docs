# Bulk Operations

These endpoints are hosted under another domain. To use them please use the following base URL: `https://api.kalendme.com`

```shell
# With shell, you can just pass the correct header with each request
curl --request GET \
  --url https://api.kalendme.com/health
```

Bulk endpoints are mainly focused towards larger organizations with hundreds of users but you can also leverage them to build customized round robin scheduling solutions among other things.

## Get Mass Availability

```shell
curl --request POST \
--url https://api.kalendme.com/v1/availability/calculate-mass-availability \
--header 'Authorization: Bearer token' \
--header 'Content-Type: application/json' \
--data '{
	"userIds":["12312313",
		"12312"],
	"startTimeMillis": 1675090800000,
	"endTimeMillis": 1676862000000,
	"durationMinutes": 30
}'
```

> The above command returns JSON structured like this:

```json
{
  "12312313": [1675090800000, 1675090800000],
  "12312": [1675090800000, 1675090800000]
}
```

This endpoint returns an array of user's availabilities based on the specified rules. You can query the availability for any amount of users and any amount of time. Although this endpoint is built for speed, the response may take longer for larger time spans or larger amounts of users.

### HTTP Request

`POST https://api.kalendme.com/v1/availability/calculate-mass-availability`

### Body Parameters

| Parameter       | Type  | Required | Description                                                                                           |
| --------------- | ----- | -------- | ----------------------------------------------------------------------------------------------------- |
| userIds         | array | Required | The ids of the users.                                                                                 |
| startTimeMillis | int   | Required | The start timestamp in epoch milliseconds to start the query for.                                     |
| endTimeMillis   | int   | Required | The end timestamp in epoch milliseconds to start the query for. It cannot span for more than a month. |
| durationMinutes | int   | Required | The duration of the session you want to schedule this user.                                           |

## Get Users Available between Two Timestamps

```shell
curl --request POST \
  --url https://api.kalendme.com/v1/availability/users-available-between \
  --header 'Authorization: Bearer token' \
  --header 'Content-Type: application/json' \
  --data '{
	"startTimeMillis": 1678158000000,
	"endTimeMillis": 1678190400000,
	"durationMinutes": 30
}'
```

> The above command returns JSON structured like this:

```json
{
  "userIds": ["12341332", "123123123"]
}
```

This endpoint returns an array of user's ids that are available based on the specified time slot. This is designed to be used as slots, so for example when you want to schedule a meeting next tuesday at 4 pm for 60 minutes and you want to know which users are available.

### HTTP Request

`POST https://api.kalendme.com/v1/availability/users-available-between`

### Body Parameters

| Parameter       | Type | Required | Description                                                                                           |
| --------------- | ---- | -------- | ----------------------------------------------------------------------------------------------------- |
| startTimeMillis | int  | Required | The start timestamp in epoch milliseconds to start the query for.                                     |
| endTimeMillis   | int  | Required | The end timestamp in epoch milliseconds to start the query for. It cannot span for more than a month. |
| durationMinutes | int  | Required | The duration of the session you want to schedule this user.                                           |

## Get Users Availabilities between Two Timestamps

```shell
curl --request POST \
  --url https://api.kalendme.com/v1/availability/users-availabilities-between \
  --header 'Authorization: Bearer token' \
  --header 'Content-Type: application/json' \
  --data '{
	"startTimeMillis": 1678244400000,
	"endTimeMillis": 1678330800000,
	"durationMinutes": 30
}'
```

> The above command returns JSON structured like this:

```json
{
  "123123123": [1678244400000, 1678244400000],
  "123123123": [1678244400000, 1678244400000]
}
```

This endpoint returns an array of user's availabilities based on the specified durationMinutes. Similar to the other endpoint but this is used when you want to various availabilities for several users at once.

### HTTP Request

`POST https://api.kalendme.com/v1/availability/users-availabilities-between`

### Body Parameters

| Parameter       | Type | Required | Description                                                                                           |
| --------------- | ---- | -------- | ----------------------------------------------------------------------------------------------------- |
| startTimeMillis | int  | Required | The start timestamp in epoch milliseconds to start the query for.                                     |
| endTimeMillis   | int  | Required | The end timestamp in epoch milliseconds to start the query for. It cannot span for more than a month. |
| durationMinutes | int  | Required | The duration of the session you want to schedule this user.                                           |
