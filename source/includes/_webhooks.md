# Webhooks

## Overview

Webhooks are used to keep your system up to date with KalendMe's information in real-time. You can configure which system events from KalendMe you want to subscribe to so that you can take action on your side with the information provided.

These are signed POST requests to a `url` of your choosing.

## Webhook Event Types

Event Type | Description | Resource Data
---------- | ----------- | -------------
`user.acceptedInvite` | Event fired when a user joins the organization (accepts an invitation to join) | [User](/#user-resource)
`event.created` | Event fired when an event is created | [Event](/#calendar-event-resource)
`event.cancelled` | Event fired when an event is cancelled | [Event](/#calendar-event-resource)

## General Body Structure

> An example webhook's POST body looks like this:

```json
{
  "id": "123213232",
  "createdAt": "2020-09-01T00:00:00.000Z",
  "type": "event.created",
  "data": {
    "id": "123",
    "userId": "321",
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

Webhooks are POST requests to your systems. All of KalendMe's requests will come in the shown format with a `Content-Type: application/json` and the following body structure describing the type of the webhook event and the data.

## Security

> Sample code snippet to verify an incoming webhook in javascript:

```javascript
import { createHash, timingSafeEqual } from 'crypto'
 
// Your server's endpoint receiving the request
export default async function(req, res) {
 const hashedBody = createHash('sha256')
   .update(`${req.body}${process.env.KALENDME_WEBHOOK_SECRET}`)
   .digest('hex')
 
 if (!timingSafeEqual(hashedBody, req.headers['Kalendme-Signature'])) {
   res.status(401).end()
   return
 }
 
 // proceed with the processing of the webhook
}
```


You’ll be provided with a webhook secret on account setup which you’ll use to calculate a hash to compare against the signature of every webhook.

All webhooks will come with a signature header: `Kalendme-Signature`

That signature has to be compared to the received body of the event.


## Webhook Resource

> An example webhook resource looks like this:

```json
{
  "webhook": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "url": "https://yourdomain.com/my-endpoint",
    "enabledEvents": [ 
      "event.created"
    ],
    "description": "My Webhook",
    "enabled": true
  }
}
```

Webhooks are all under your organization. Webhooks are formed by the following fields.

Parameter | Type | Description
--------- | ---- | -----------
id | string | The resource's id
createdAt | timestamp | The resource's creation timesstamp
updatedAt | timestamp | The resource's last updated timestamp
url | string | The url of your endpoint that will listen to events coming in
enabledEvents | string[] | An array containing the [webhook event types](/#webhook-event-types) you want to subscribe to
decription | string | A description of ths webhook
enabled | boolean | Controls whether this webhook actually emits events or not

## Create a Webhook

```shell
curl "https://www.kalendme.com/api/v1/webhooks" \
  -X POST
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "url": "https://yourdomain.com/my-endpoint",
    "enabledEvents": [ 
      "event.created"
    ],
    "description": "My Webhook"
  }'
```

> The above command returns JSON structured like this:

```json
{
  "webhook": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "url": "https://yourdomain.com/my-endpoint",
    "enabledEvents": [ 
      "event.created"
    ],
    "description": "My Webhook",
    "enabled": true
  }
}
```

This endpoint creates a new webhook under your organization.

### HTTP Request

`POST https://www.kalendme.com/api/v1/webhooks`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
url | string | Required | The url of your endpoint that will listen to events coming in
enabledEvents | string[] | Required | An array containing the [webhook event types](/#webhook-event-types) you want to subscribe to
decription | string | Optional | A description of ths webhook
enabled | boolean | Optional | `Default: true` Controls whether this webhook actually emits events or not

## Get All Webhooks

```shell
curl "https://www.kalendme.com/api/v1/webhooks" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "webhooks": [
    {
      "id": "123213232",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "url": "https://yourdomain.com/my-endpoint",
      "enabledEvents": [ 
        "event.created"
      ],
      "description": "My Webhook",
      "enabled": true
    },
    {
      "id": "123213233",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "url": "https://yourdomain.com/my-endpoint-2",
      "enabledEvents": [ 
        "event.cancelled"
      ],
      "description": "My Cancelled Events Webhook",
      "enabled": true
    }
  ]
}

```

This endpoint retrieves all webhooks for your organization.

### HTTP Request

`GET https://www.kalendme.com/api/v1/webhooks`

## Get a Specific Webhook

```shell
curl "https://www.kalendme.com/api/v1/webhooks/123213232" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "webhook": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "url": "https://yourdomain.com/my-endpoint",
    "enabledEvents": [ 
      "event.created"
    ],
    "description": "My Webhook",
    "enabled": true
  }
}
```

This endpoint retrieves a specific webhook.

### HTTP Request

`GET https://www.kalendme.com/api/v1/webhooks/<webhookId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
webhookId | string | Required | The ID of the webhook to retrieve

## Get a Webhook's Logs

```shell
curl "https://www.kalendme.com/api/v1/webhooks/123213232/logs" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{ 
  "webhookEventsLogs": [
    {
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "webhookId": "12312313",
      "statusCode": 200,
      "requestData": {
        "id": "310675722544874052",
        "createdAt": "2021-09-25T00:58:26.866-03:00",
        "type": "event.created",
        "data": {
          "id": "310663991379624516",
          "startTimestamp": 1633834800000,
          "title": "Test Event",
          "durationMinutes": 30,
          "guests": [
            "raimille2@gmail.com"
          ],
          "mainGuestTimeZone": "America/Sao_Paulo",
          "mainGuestName": "Rai",
          "mainGuestLanguage": "es",
          "status": "CONFIRMED"
        }
      },
      "responseText": "",
      "responseJson": {},
      "id": "123123"
    }
  ]
}

```

This endpoint retrieves a specific webhook's logs. Contains response status code, text and json contents if available. 

<aside class="notice">
  Webhook events logs are retained for 15 days only.
</aside>

### Webhook Event Log Resource

Parameter | Type | Description
--------- | ---- | -----------
id | string | The resource's id
createdAt | timestamp | The resource's creation timesstamp
updatedAt | timestamp | The resource's last updated timestamp
webhookId | string | The id of the webhook this log entry belongs to
statusCode | int | The http status code your system replied with when this event was emitted
requestData | string | The JSON data sent over to your system with this event
responseJson | object | The JSON response from your system if replied with content type of `application/json`
responseText | string | The TEXT response from your system if replied with content type other than `application/json`

### HTTP Request

`GET https://www.kalendme.com/api/v1/webhooks/<webhookId>/logs`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
webhookId | string | Required | The ID of the webhook to retrieve

## Update a Webhook

```shell
curl "https://www.kalendme.com/api/v1/webhooks/123213232" \
  -X PATCH
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "enabled": false
  }'
```

> The above command returns JSON structured like this:

```json
{
  "webhook": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "url": "https://yourdomain.com/my-endpoint",
    "enabledEvents": [ 
      "event.created"
    ],
    "description": "My Webhook",
    "enabled": false
  }
}
```

This endpoint updates a webhook under your organization. Used to switch out the `url` or disable it by setting the `enabled` flag to false.
 
### HTTP Request

`PATCH https://www.kalendme.com/api/v1/webhooks/<webhookId>`

### Body Parameters 

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
url | string | Optional | The url of your endpoint that will listen to events coming in
enabledEvents | string[] | Optional | An array containing the [webhook event types](/#webhook-event-types) you want to subscribe to
decription | string | Optional | A description of ths webhook
enabled | boolean | Optional | Controls whether this webhook actually emits events or not

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
webhookId | string | Required | The ID of the webhook to update

## Delete a Specific Webhook

```shell
curl "https://www.kalendme.com/api/v1/webhooks/123213232" \
  -X DELETE
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "webhook": {
    "id": "123213232",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "url": "https://yourdomain.com/my-endpoint",
    "enabledEvents": [ 
      "event.created"
    ],
    "description": "My Webhook",
    "enabled": false
  }
}
```

This endpoint deletes a specific webhook from your organization.

### HTTP Request

`DELETE https://www.kalendme.com/api/v1/webhooks/<webhookId>`

### URL Parameters

Parameter | Type | Required | Description
--------- | ---- | -------- | -----------
webhookId | string | Required | The ID of the webhook to delete