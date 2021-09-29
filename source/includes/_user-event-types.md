# User Event Types

## User Event Type Resource

> An example user event type model looks like this:

```json
{
  "userEventType": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "userId": "112323321",
    "title": "Quick Meeting",
    "durationMinutes": 30,
    "urlString": "quick-meeting",
    "description": "A quick session to catch up",
    "minimumNotice": 0,
    "active": true,
    "public": true,
    "weekAvailability": [],
    "customQuestions": [
      {
        "id": "3656475289d4aecd03804ec4d6045953", // An ID of your choosing to map to your system
        "order": 0,
        "question": "What's your phone number?"
      }
    ]
  }
}
```

User event types are the different types of events a user can have booked. They have pre-defined parameters like duration or specific minimum notice times. They can be accessed by a user's `urlString` followed by the User Event Type's `urlString` (example: `kalendme.com/john/quick-meeting`). User event types are formed by the following fields.

Parameter | Type | Description
--------- | ---- | -----------
id | string | The resource's id
createdAt | timestamp | The resource's creation timesstamp
updatedAt | timestamp | The resource's last updated timestamp
userId | string | The ID of the user that this event type belongs to.
title | string | The title of the event. For example, `Interview`
description | string | The description of the event
durationMinutes | int | Required | The duration in minutes of the event.
urlString | string | A unique URL for the user's event type. Used for their bookings page such as kalendme.com/`john-123`/`event-type`. If none is specified, a random one will be generated on user event type creation.
minimumNoticeMinutes | int | The minimum number of minutes notice for a user event type. For example, `120` if this event type requires a minimum of 2 hours of notice to be booked in advance. 
enabled | boolean | `Default: true` Specifies whether this user event type is enabled or not. Disabled event types cannot be booked or found through their `urlString`  
public | boolean | `Default: true` Specifies whether this user event type is public or not. Making an event non-public hides it from a user's main urlString.  
weekAvailability | [WeekAvailability](/#week-availability) | An object containing an override to the user's general avialability. A specific user event types's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times.
customQuestions | [CustomQuestion](/#custom-question)[] | An array containing custom questions to ask user's when booking this event type.

## Create a User Event Type

```shell
curl "https://www.kalendme.com/api/v1/users/112323321/event-types" \
  -X POST
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "title": "Quick Meeting",
    "durationMinutes": 30,
    "urlString": "quick-meeting",
    "description": "A quick session to catch up",
    "weekAvailability": [],
    "customQuestions": [
      {
        "id": "3656475289d4aecd03804ec4d6045953", // An ID of your choosing to map to your system
        "order": 0,
        "question": "Whats your phone number?"
      }
    ]
  }'
```

> The above command returns JSON structured like this:

```json
{
  "userEventType": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "userId": "112323321",
    "title": "Quick Meeting",
    "durationMinutes": 30,
    "urlString": "quick-meeting",
    "description": "A quick session to catch up",
    "minimumNotice": 0,
    "active": true,
    "public": true,
    "weekAvailability": [],
    "customQuestions": [
      {
        "id": "3656475289d4aecd03804ec4d6045953", // An ID of your choosing to map to your system
        "order": 0,
        "question": "What's your phone number?"
      }
    ]
  }
}
```

This endpoint creates a new event type for a user.

### HTTP Request

`POST https://www.kalendme.com/api/v1/users/<userId>/event-types`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
title | string | Required | The title of the event. For example, `Interview`
description | string | Optional | The description of the event. Can be blank.
durationMinutes | int | Required | The duration in minutes of the event.
urlString | string | Optional | A unique URL for the user's event type. Used for their bookings page such as kalendme.com/`john-123`/`event-type`. If none is specified, a random one will be generated on user event type creation.
minimumNoticeMinutes | int | Optional | The minimum number of minutes notice for a user event type. For example, `120` if this event type requires a minimum of 2 hours of notice to be booked in advance. 
enabled | boolean | Optional | `Default: true` Specifies whether this user event type is enabled or not. Disabled event types cannot be booked or found through their `urlString`  
public | boolean | Optional | `Default: true` Specifies whether this user event type is public or not. Making an event non-public hides it from a user's main urlString.  
weekAvailability | Optional | [WeekAvailability](/#week-availability) | An object containing an override to the user's general avialability. A specific user event types's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times.
customQuestions | Optional | [CustomQuestion](/#custom-question)[] | An array containing custom questions to ask user's when booking this event type.

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this event type will belong to.

## Get a User's Event Types 

```shell
curl "https://www.kalendme.com/api/v1/users/123213233/event-types" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "userEventTypes": [
    {
      "id": "123213232",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "userId": "112323321",
      "title": "Quick Meeting",
      "durationMinutes": 30,
      "urlString": "quick-meeting",
      "description": "A quick session to catch up",
      "minimumNotice": 0,
      "active": true,
      "public": true,
      "weekAvailability": [],
      "customQuestions": [
        {
          "id": "3656475289d4aecd03804ec4d6045953", // An ID of your choosing to map to your system
          "order": 0,
          "question": "What's your phone number?"
        }
      ]
    }, // ...
  ]
}

```

This endpoint retrieves all user event types for a specific user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/event-types`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user these event types belong to.

## Get a Specific User Event Type

```shell
curl "https://www.kalendme.com/api/v1/users/112323321/event-types/123213232" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "userEventType": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "userId": "112323321",
    "title": "Quick Meeting",
    "durationMinutes": 30,
    "urlString": "quick-meeting",
    "description": "A quick session to catch up",
    "minimumNotice": 0,
    "active": true,
    "public": true,
    "weekAvailability": [],
    "customQuestions": [
      {
        "id": "3656475289d4aecd03804ec4d6045953", // An ID of your choosing to map to your system
        "order": 0,
        "question": "What's your phone number?"
      }
    ]
  }
}
```

This endpoint retrieves a specific user's event type.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/event-types/<eventTypeId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this event type belong to.
eventTypeId | string | Required | The id of the user event type.

## Update a User Event Type

```shell
curl "https://www.kalendme.com/api/v1/users/112323321/event-types/123213232" \
  -X PATCH
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "public": false,
    "customQuestions: []
  }'
  -
```

> The above command returns JSON structured like this:

```json
{
  "userEventType": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "userId": "112323321",
    "title": "Quick Meeting",
    "durationMinutes": 30,
    "urlString": "quick-meeting",
    "description": "A quick session to catch up",
    "minimumNotice": 0,
    "active": true,
    "public": false,
    "weekAvailability": [],
    "customQuestions": []
  }
}
```

This endpoint updates a user's event type.

### HTTP Request

`PATCH https://www.kalendme.com/api/v1/users/<userId>/event-types/<eventTypeId>`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
title | string | Optional | The title of the event. For example, `Interview`
description | string | Optional | The description of the event. Can be blank.
durationMinutes | int | Optional | The duration in minutes of the event.
urlString | string | Optional | A unique URL for the user's event type. Used for their bookings page such as kalendme.com/`john-123`/`event-type`. If none is specified, a random one will be generated on user event type creation.
minimumNoticeMinutes | int | Optional | The minimum number of minutes notice for a user event type. For example, `120` if this event type requires a minimum of 2 hours of notice to be booked in advance. 
enabled | boolean | Optional | Specifies whether this user event type is enabled or not. Disabled event types cannot be booked or found through their `urlString`  
public | boolean | Optional | Specifies whether this user event type is public or not. Making an event non-public hides it from a user's main urlString.  
weekAvailability | Optional | [WeekAvailability](/#week-availability) | An object containing an override to the user's general avialability. A specific user event types's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times.
customQuestions | Optional | [CustomQuestion](/#custom-question)[] | An array containing custom questions to ask user's when booking this event type.

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this event type belong to.
eventTypeId | string | Required | The id of the user event type.


## Delete a Specific User Event Type

```shell
curl "https://www.kalendme.com/api/v1/users/112323321/event-types/123213232" \
  -X DELETE
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "userEventType": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "userId": "112323321",
    "title": "Quick Meeting",
    "durationMinutes": 30,
    "urlString": "quick-meeting",
    "description": "A quick session to catch up",
    "minimumNotice": 0,
    "active": true,
    "public": false,
    "weekAvailability": [],
    "customQuestions": []
  }
}
```

This endpoint deletes a specific user event type.

### HTTP Request

`DELETE https://www.kalendme.com/api/v1/users/<userId>/event-types/<eventTypeId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this event type belong to.
eventTypeId | string | Required | The id of the user event type.