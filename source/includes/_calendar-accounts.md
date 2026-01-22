# Calendar Accounts

## Calendar Account Resource

> An example calendar account model looks like this:

```json
{
  "account": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "connectionStatus": "OK",
    "provider": "google"
  }
}
```

Calendar accounts belong to users, they are unique calendar account providers that can contain several calendars inside them. Each calendar inside an account can be used to calculate a user's availability. Each KalendMe user comes with a KALENDME Calendr Account which are needed if you want that user to use a KalendMe calendar without needing to connect their Google/Microsoft accounts. Calendar Accounts are formed by the following fields.

| Parameter        | Type      | Description                                                                    |
| ---------------- | --------- | ------------------------------------------------------------------------------ |
| id               | string    | The resource's id                                                              |
| createdAt        | timestamp | The resource's creation timesstamp                                             |
| updatedAt        | timestamp | The resource's last updated timestamp                                          |
| userId           | string    | The ID of the user that this calendar belongs to.                              |
| provider         | string    | The provider, today we support "google", "microsoft" and "kalendme".           |
| connectionStatus | string    | Specifies whether the calendar account needs reconnection or is OK to be used. |

## Connect a Calendar Account

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/accounts" \
  -X POST
  -H "Authorization: Bearer abcdef123456"
  -H "Content-Type: application/json"
  -d '{
    "onSuccessRedirectUrl": "https://www.example.com"
  }'
```

### Body Parameters

| Parameter         | Type                              | Required | Description                                                   |
| ----------------- | --------------------------------- | -------- | ------------------------------------------------------------- |
| onSuccessRedirectUrl             | string                            | Optional | The URL to redirect to after the calendar account is connected. The domain must be whitelisted in your KalendMe account settings.              |

> The above command returns JSON structured like this:

```json
{
  "connectGoogleAccountUrl": "https://www.kalendme.com/verylongtoken",
  "connectMicrosoftAccountUrl": "https://www.kalendme.com/verylongtoken"
}
```

This endpoint provides you with two magic links to provide to the user. One to connect a new Microsft account and another to connect a new Google account. The user will be redirected to the provider's authentication page and then redirected back to KalendMe showing that everything worked with your configured branding.

### HTTP Request

`POST https://www.kalendme.com/api/v1/users/<userId>/accounts`

## Get a user's Calendar Accounts

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/accounts" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "accounts": [
    {
      "id": "123213232",
      "userId": "123123123",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "provider": "kalendme",
      "connectionStatus": "OK"
    },
    {
      "id": "123213232",
      "userId": "123123123",
      "createdAt": "2020-09-01T00:00:00.000Z",
      "updatedAt": "2020-09-01T00:00:00.000Z",
      "provider": "google",
      "connectionStatus": "OK"
    }
  ]
}
```

This endpoint retrieves all calendar accounts for a user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/calendar-accounts`

### URL Parameters

| Parameter | Type   | Required | Description                                        |
| --------- | ------ | -------- | -------------------------------------------------- |
| userId    | string | Required | The ID of the user's calendar accounts to retrieve |

## Get a Specific Calendar Account

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/accounts/123213232" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "account": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "provider": "google",
    "connectionStatus": "OK"
  }
}
```

This endpoint retrieves a specific calendar account for a user.

### HTTP Request

`GET https://www.kalendme.com/api/v1/users/<userId>/accounts/<accountId>`

### URL Parameters

| Parameter | Type   | Required | Description                                        |
| --------- | ------ | -------- | -------------------------------------------------- |
| userId    | string | Required | The ID of the user's calendar accounts to retrieve |
| accountId | string | Required | The ID of the calendar account to retrieve         |

## Delete a Specific Calendar Account

```shell
curl "https://www.kalendme.com/api/v1/users/123123123/accounts/123213232" \
  -X POST \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "account": {
    "id": "123213232",
    "userId": "123123123",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "provider": "google",
    "connectionStatus": "OK"
  }
}
```

This endpoint retrieves a specific calendar account for a user.

### HTTP Request

`DELETE https://www.kalendme.com/api/v1/users/<userId>/accounts/<accountId>`

### URL Parameters

| Parameter | Type   | Required | Description                                      |
| --------- | ------ | -------- | ------------------------------------------------ |
| userId    | string | Required | The ID of the user's calendar accounts to delete |
| accountId | string | Required | The ID of the calendar account to delete         |
