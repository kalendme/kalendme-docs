# Calendar Events

## Calendar Event Resource

> An example calendar event model looks like this:

```json
{
  "calendarEvent": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "title": "Quick Meeting",
    "description": "Let's discuss a quick topic",
    "startTimestamp": 1634099600000,
    "startTimeUtc": "2020-09-01T00:00:00.000Z",
    "durationMinutes": 30,
    "location": "https://myvideoconferencing.com/my-link"
    "guests": [
      "michael@dundermifflin.com"
    ],
    "mainGuestName": "Pam Beesly",
    "mainGuestTimeZone": "America/New_York",
    "mainGuestLanguage": "en",
    "status": "CONFIRMED"
  }
}
```

Calendar events belong to users, they are unique instances in time that a user is booked for. Having a calendar event blocks that user's availability for the duration of the event. Calendar Events are formed by the following fields.

| Parameter         | Type                              | Description                                                                                                                                                                        |
| ----------------- | --------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| id                | string                            | The resource's id                                                                                                                                                                  |
| createdAt         | timestamp                         | The resource's creation timesstamp                                                                                                                                                 |
| updatedAt         | timestamp                         | The resource's last updated timestamp                                                                                                                                              |
| userId            | string                            | The ID of the user that this calendar event belongs to.                                                                                                                            |
| title             | string                            | The title of the event. For example, `Interview`                                                                                                                                   |
| description       | string                            | The description of the event                                                                                                                                                       |
| startTimestamp    | long                              | The timestamp in epoch milliseconds of when this event starts                                                                                                                      |
| durationMinutes   | int                               | The duration in minutes of the event                                                                                                                                               |
| guests            | string[]                          | An array of the guest's emails for this event                                                                                                                                      |
| mainGuestName     | string                            | The name of the person that booked the event                                                                                                                                       |
| mainGuestTimeZone | string                            | The time zone of the person that booked the event                                                                                                                                  |
| mainGuestLanguage | string                            | The language of the person that booked the event                                                                                                                                   |
| status            | string                            | The status of the event. Can be `CONFIRMED` or `CANCELLED`.                                                                                                                        |
| padding           | [EventPadding](/#event-padding)   | Used to specify if an event needs time padding before and/or after to be scheduled. Must be numbers in minutes of the padding needed and values can be anywhere between 0 and 180. |
| location          | [EventLocation](/#event-location) | See Special Models definitions, this defines the event's location.                                                                                                                 |

## Create a Calendar Event

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendar-events?sendGuestNotificationEmails=true" \
  -X POST
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "title": "Quick Meeting",
    "startTimestamp": 1634099600000,
    "durationMinutes": 30,
    "guests": [
      "michael@dundermifflin.com"
    ],
    "mainGuestName": "Pam Beesly",
    "mainGuestTimeZone": "America/New_York",
    "mainGuestLanguage": "en"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "calendarEvent": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "title": "Quick Meeting",
    "description": "Let's discuss a quick topic",
    "startTimestamp": 1634099600000,
    "startTimeUtc": "2020-09-01T00:00:00.000Z",
    "durationMinutes": 30,
    "guests": ["michael@dundermifflin.com"],
    "mainGuestName": "Pam Beesly",
    "mainGuestTimeZone": "America/New_York",
    "mainGuestLanguage": "en",
    "status": "CONFIRMED",
    "location": {
      "type": "other"
    }
  }
}
```

This endpoint creates a new calendar event for a user. In other words, it books that user during that time. To control which calendar account this event is written to, you have to configure that user's `output` calendar account connection.

### HTTP Request

`POST https://www.kalendme.com/api/v1/users/<userId>/calendar-events`

### Body Parameters

| Parameter         | Type                              | Required | Description                                                   |
| ----------------- | --------------------------------- | -------- | ------------------------------------------------------------- |
| title             | string                            | Required | The title of the event. For example, `Interview`              |
| startTimestamp    | long                              | Required | The timestamp in epoch milliseconds of when this event starts |
| durationMinutes   | int                               | Required | The duration in minutes of the event                          |
| guests            | string[]                          | Required | An array of the guest's emails for this event                 |
| mainGuestName     | string                            | Required | The name of the person that booked the event                  |
| mainGuestTimeZone | string                            | Required | The time zone of the person that booked the event             |
| mainGuestLanguage | string                            | Required | The language of the person that booked the event              |
| description       | string                            | Optional | The description of the event                                  |
| location          | [EventLocation](/#event-location) | Optional | The location of this event                                    |
| padding           | [EventPadding](/#event-padding)   | Optional | Any padding needed before or after scheduling this event      |

### URL Parameters

| Parameter                      | Type    | Required | Description                                                                                                                                                  |
| ------------------------------ | ------- | -------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| userId                         | string  | Required | The id of the user this calendar event will belong to.                                                                                                       |
| sendGuestNotificationEmails    | boolean | Optional | `Default: false` Whether you want to send an email notification that this event was created to the `guests`.                                                 |
| sendOrganizerNotificationEmail | boolean | Optional | `Default: false` Whether you want to send an email notification that this event was created to the event organizer (the user who's `userId` you're booking). |
| linkId                         | string  | Optional | `Default: null` If you want to apply an event's padding rules from a link, use this to specify which one                                                     |

## Get a Calendar Event

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendar-events/123213232" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "calendarEvent": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "title": "Quick Meeting",
    "description": "Let's discuss a quick topic",
    "startTimestamp": 1634099600000,
    "startTimeUtc": "2020-09-01T00:00:00.000Z",
    "durationMinutes": 30,
    "guests": ["michael@dundermifflin.com"],
    "mainGuestName": "Pam Beesly",
    "mainGuestTimeZone": "America/New_York",
    "mainGuestLanguage": "en",
    "status": "CONFIRMED",
    "location": {
      "type": "other"
    }
  }
}
```

This endpoint gets a specific calendar event for a user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/calendar-events/<eventId>`

### URL Parameters

| Parameter | Type   | Required | Description                                     |
| --------- | ------ | -------- | ----------------------------------------------- |
| userId    | string | Required | The ID of the user's calendar event to retrieve |
| eventId   | string | Required | The ID of the calendar event to retrieve        |

## Get Calendar Events between timestamps

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendar-events?startTimestamp=0&endTimestamp=100000" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "calendarEvents": [
    {
      "id": "123213232",
      "userId": "123123123",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "title": "Quick Meeting",
      "description": "Let's discuss a quick topic",
      "startTimestamp": 1634099600000,
      "startTimeUtc": "2020-09-01T00:00:00.000Z",
      "durationMinutes": 30,
      "guests": ["michael@dundermifflin.com"],
      "mainGuestName": "Pam Beesly",
      "mainGuestTimeZone": "America/New_York",
      "mainGuestLanguage": "en",
      "status": "CONFIRMED",
      "location": {
        "type": "other"
      }
    } // ...
  ]
}
```

This endpoint gets a list of calendar events for a user between a start and an end timestamp.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/calendar-events?startTimestamp=&endTimestamp=`

### URL Parameters

| Parameter      | Type   | Required | Description                                                                                           |
| -------------- | ------ | -------- | ----------------------------------------------------------------------------------------------------- |
| userId         | string | Required | The ID of the user's calendar event to retrieve                                                       |
| startTimestamp | int    | Required | The start timestamp in epoch milliseconds to start the query for.                                     |
| endTimestamp   | int    | Required | The end timestamp in epoch milliseconds to start the query for. It cannot span for more than a month. |

## Update a Calendar Event

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/calendar-events/123213232" \
  -X PATCH
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "status": "CANCELLED"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "calendarEvent": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "title": "Quick Meeting",
    "description": "Let's discuss a quick topic",
    "startTimestamp": 1634099600000,
    "startTimeUtc": "2020-09-01T00:00:00.000Z",
    "durationMinutes": 30,
    "guests": ["michael@dundermifflin.com"],
    "mainGuestName": "Pam Beesly",
    "mainGuestTimeZone": "America/New_York",
    "mainGuestLanguage": "en",
    "status": "CANCELLED",
    "location": {
      "type": "other"
    }
  }
}
```

This endpoint updates a calendar event. Used for rescheduling or cancelling calendar events.

### HTTP Request

`PATCH https://www.kalendme.com/api/v1/users/<userId>/calendar-events/<eventId>`

### Body Parameters

| Parameter       | Type     | Required | Description                                                   |
| --------------- | -------- | -------- | ------------------------------------------------------------- |
| title           | string   | Optional | The title of the event. For example, `Interview`              |
| description     | string   | Optional | The description of the event                                  |
| startTimestamp  | long     | Optional | The timestamp in epoch milliseconds of when this event starts |
| durationMinutes | int      | Optional | The duration in minutes of the event                          |
| guests          | string[] | Optional | An array of the guest's emails for this event                 |
| status          | string   | Optional | The status of the event. Can be `CONFIRMED` or `CANCELLED`.   |

### URL Parameters

| Parameter | Type   | Required | Description                                   |
| --------- | ------ | -------- | --------------------------------------------- |
| userId    | string | Required | The ID of the user's calendar event to update |
| eventId   | string | Required | The ID of the calendar event to update        |
