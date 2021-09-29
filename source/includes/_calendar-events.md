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
    "durationMinutes": 30,
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

Parameter | Type | Description
--------- | ---- | -----------
id | string | The resource's id
createdAt | timestamp | The resource's creation timesstamp
updatedAt | timestamp | The resource's last updated timestamp
userId | string | The ID of the user that this calendar event belongs to.
title | string | The title of the event. For example, `Interview`
description | string | The description of the event
startTimestamp | long | The timestamp in epoch milliseconds of when this event starts
durationMinutes | int | The duration in minutes of the event
guests | string[] | An array of the guest's emails for this event
mainGuestName | string | The name of the person that booked the event
mainGuestTimeZone | string | The time zone of the person that booked the event
mainGuestLanguage | string | The language of the person that booked the event
status | string | The status of the event. Can be `CONFIRMED` or `CANCELLED`. 

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
    "durationMinutes": 30,
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

This endpoint creates a new calendar event for a user. In other words, it books that user during that time. To control which calendar account this event is written to, you have to configure that user's `output` calendar account connection.

### HTTP Request

`POST https://www.kalendme.com/api/v1/users/<userId>/calendar-events`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
title | string | The title of the event. For example, `Interview`
description | string | The description of the event
startTimestamp | long | The timestamp in epoch milliseconds of when this event starts
durationMinutes | int | The duration in minutes of the event
guests | string[] | An array of the guest's emails for this event
mainGuestName | string | The name of the person that booked the event
mainGuestTimeZone | string | The time zone of the person that booked the event
mainGuestLanguage | string | The language of the person that booked the event

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this calendar event will belong to.
sendGuestNotificationEmails | boolean | Optional | `Default: false` Whether you want to send an email notification that this event was created to the `guests`.
sendOrganizerNotificationEmail | boolean | Optional | `Default: false` Whether you want to send an email notification that this event was created to the event organizer (the user who's `userId` you're booking).

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
    "durationMinutes": 30,
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

This endpoint gets a specific calendar event for a user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/calendar-events/<eventId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The ID of the user's calendar event to retrieve
eventId | string | Required | The ID of the calendar event to retrieve

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
    "durationMinutes": 30,
    "guests": [
      "michael@dundermifflin.com"
    ],
    "mainGuestName": "Pam Beesly",
    "mainGuestTimeZone": "America/New_York",
    "mainGuestLanguage": "en",
    "status": "CANCELLED"
  }
}
```

This endpoint updates a calendar event. Used for rescheduling or cancelling calendar events.

### HTTP Request

`PATCH https://www.kalendme.com/api/v1/users/<userId>/calendar-events/<eventId>`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
title | string | Optional | The title of the event. For example, `Interview`
description | string | Optional | The description of the event
startTimestamp | long | Optional | The timestamp in epoch milliseconds of when this event starts
durationMinutes | int | Optional | The duration in minutes of the event
guests | string[] | Optional | An array of the guest's emails for this event
status | string | Optional | The status of the event. Can be `CONFIRMED` or `CANCELLED`. 

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The ID of the user's calendar event to update
eventId | string | Required | The ID of the calendar event to update
