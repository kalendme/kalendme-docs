# User Links

## User Link Resource

> An example user link model looks like this:

```json
{
  "link": {
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
    "visible": true,
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

Links are the different types of events or rules that a user can be booked through. They have pre-defined parameters like duration or specific minimum notice times. They can be accessed by a user's `urlString` followed by the Link's `urlString` (example: `kalendme.com/john/quick-meeting`). Links are formed by the following fields.

Parameter | Type | Description
--------- | ---- | -----------
id | string | The resource's id
createdAt | timestamp | The resource's creation timesstamp
updatedAt | timestamp | The resource's last updated timestamp
userId | string | The ID of the user that this link belongs to
title | string | The title of the event this link will create. For example, `Interview`
description | string | The description of the event this link will create
durationMinutes | int | Required | The duration in minutes of the event this link will create.
urlString | string | A unique URL for the user's link. Used for their bookings page such as kalendme.com/`john-123`/`link-url`. If none is specified, a random one will be generated on link creation.
minimumNoticeMinutes | int | The minimum number of minutes notice required to book through this link. For example, `120` if this link type requires a minimum of 2 hours of notice to be booked in advance
enabled | boolean | `Default: true` Specifies whether this link is enabled or not. Disabled links cannot be booked or found through their `urlString`  
visible | boolean | `Default: true` Specifies whether this link is visible or not. Making a link non-visible hides it from a user's main urlString.  
weekAvailability | [WeekAvailability](/#week-availability) | An object containing an override to the user's general avialability. A specific link's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times.
customQuestions | [CustomQuestion](/#custom-question)[] | An array containing custom questions to ask user's when booking this link.

## Create a Link

```shell
curl "https://www.kalendme.com/api/v1/users/112323321/links" \
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
  "link": {
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
    "visible": true,
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

This endpoint creates a new link for a user.

### HTTP Request

`POST https://www.kalendme.com/api/v1/users/<userId>/links`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
title | string | Required | The title of the event this link will create. For example, `Interview`
description | string | Optional | The description of the event this link will create. Can be blank.
durationMinutes | int | Required | The duration in minutes of the event this link will create.
urlString | string | Optional | A unique URL for the user's link. Used for their bookings page such as kalendme.com/`john-123`/`link-url`. If none is specified, a random one will be generated on link creation.
minimumNoticeMinutes | int | Optional | The minimum number of minutes notice required to book through this link. For example, `120` if this link type requires a minimum of 2 hours of notice to be booked in advance.
enabled | boolean | Optional | `Default: true` Specifies whether this link is enabled or not. Disabled links cannot be booked or found through their `urlString`  
visible | boolean | Optional | `Default: true` Specifies whether this link is visible or not. Making a link non-visible hides it from a user's main urlString.  
weekAvailability | Optional | [WeekAvailability](/#week-availability) | An object containing an override to the user's general avialability. A specific link's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times.
customQuestions | Optional | [CustomQuestion](/#custom-question)[] | An array containing custom questions to ask user's when booking through this link.

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this link will belong to.

## Get a User's Links

```shell
curl "https://www.kalendme.com/api/v1/users/123213233/links" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "links": [
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
      "visible": true,
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

This endpoint retrieves all user links for a specific user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/links`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user these links belong to.

## Get a Specific User Link

```shell
curl "https://www.kalendme.com/api/v1/users/112323321/links/123213232" \
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
    "visible": true,
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

This endpoint retrieves a specific user's link.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/links/<linkId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this link belong to.
linkId | string | Required | The id of the user link.

## Update a User link

```shell
curl "https://www.kalendme.com/api/v1/users/112323321/links/123213232" \
  -X PATCH
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "visible": false,
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
    "visible": false,
    "weekAvailability": [],
    "customQuestions": []
  }
}
```

This endpoint updates a user's link.

### HTTP Request

`PATCH https://www.kalendme.com/api/v1/users/<userId>/links/<linkId>`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
title | string | Optional | The title of the event. For example, `Interview`
description | string | Optional | The description of the event. Can be blank.
durationMinutes | int | Optional | The duration in minutes of the event.
urlString | string | Optional | A unique URL for the user's link. Used for their bookings page such as kalendme.com/`john-123`/`event-type`. If none is specified, a random one will be generated on user link creation.
minimumNoticeMinutes | int | Optional | The minimum number of minutes notice required to book through this link. For example, `120` if this link type requires a minimum of 2 hours of notice to be booked in advance
enabled | boolean | Optional | Specifies whether this user link is enabled or not. Disabled links cannot be booked or found through their `urlString`  
visible | boolean | Optional | Specifies whether this user link is visible or not. Making an event non-visible hides it from a user's main urlString.  
weekAvailability | Optional | [WeekAvailability](/#week-availability) | An object containing an override to the user's general avialability. A specific user link's week availability from Sunday [0] through Saturday [7] and each day's availability slots with a `start` and `end` times.
customQuestions | Optional | [CustomQuestion](/#custom-question)[] | An array containing custom questions to ask user's when booking through this link.

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this link belong to.
linkId | string | Required | The id of the user link.


## Delete a Specific User Link

```shell
curl "https://www.kalendme.com/api/v1/users/112323321/links/123213232" \
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
    "visible": false,
    "weekAvailability": [],
    "customQuestions": []
  }
}
```

This endpoint deletes a specific user link.

### HTTP Request

`DELETE https://www.kalendme.com/api/v1/users/<userId>/links/<linkId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
userId | string | Required | The id of the user this link belong to.
linkId | string | Required | The id of the user link.