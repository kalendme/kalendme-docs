# Organization

## Organization Resource

> An example organization model looks like this:

```json
{
  "organization": {
    "id": "org_123123123",
    "name": "Acme Inc.",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "settings": {
      "safeRedirectDomains": ["https://app.example.com"],
      "isAnnonymousInvitesModeEnabled": false,
      "customEmailSettings": {
        "domain": "mail.example.com",
        "from": "hello@example.com",
        "key": "..."
      }
    }
  }
}
```

The organization is the top-level tenant that owns your users, links, and API key. Its `settings` object holds account-wide configuration. The settings keys listed below are the ones exposed and editable through the API.

### Editable settings

| Setting                          | Type       | Description                                                                                                                               |
| -------------------------------- | ---------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| safeRedirectDomains              | string[]   | Allowlist of URL prefixes accepted as `onSuccessRedirectUrl` when connecting a calendar account. Each entry must be a valid http/https URL (e.g. `https://app.example.com`). A redirect is accepted only if it starts with one of these entries. |
| isAnnonymousInvitesModeEnabled   | boolean    | Whether anonymous invite mode is enabled for the organization.                                                                          |
| customEmailSettings              | object     | Custom sender configuration (`domain`, `from`, `key`) for transactional email.                                                          |

## Get your Organization

```shell
curl "https://www.kalendme.com/api/v1/organization" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "organization": {
    "id": "org_123123123",
    "name": "Acme Inc.",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "settings": {
      "safeRedirectDomains": ["https://app.example.com"],
      "isAnnonymousInvitesModeEnabled": false,
      "customEmailSettings": {
        "domain": "mail.example.com",
        "from": "hello@example.com",
        "key": "..."
      }
    }
  }
}
```

This endpoint returns your organization. Secret fields such as your API key and webhook secret are omitted from the response.

### HTTP Request

`GET https://www.kalendme.com/api/v1/organization`

## Update your Organization settings

```shell
curl "https://www.kalendme.com/api/v1/organization" \
  -X PATCH \
  -H "Authorization: Bearer abcdef123456" \
  -H "Content-Type: application/json" \
  -d '{
    "settings": {
      "safeRedirectDomains": ["https://app.example.com"]
    }
  }'
```

> The above command returns the updated organization:

```json
{
  "organization": {
    "id": "org_123123123",
    "name": "Acme Inc.",
    "createdAt": "2020-09-01T00:00:00.000Z",
    "updatedAt": "2020-09-01T00:00:00.000Z",
    "settings": {
      "safeRedirectDomains": ["https://app.example.com"],
      "isAnnonymousInvitesModeEnabled": false,
      "customEmailSettings": {
        "domain": "mail.example.com",
        "from": "hello@example.com",
        "key": "..."
      }
    }
  }
}
```

Updates one or more of the editable settings keys. This is a **shallow merge**: only the keys you send are replaced; any other settings keys keep their current value. To clear a key, send it explicitly with an empty value (e.g. `"safeRedirectDomains": []`).

Only the editable keys documented above are accepted. Sending any settings key other than those listed above is rejected with HTTP 400. When `safeRedirectDomains` is provided it must be an array of valid http/https URLs, otherwise the request fails with error code `1075`.

### Body Parameters

| Parameter | Type   | Required | Description                                                        |
| --------- | ------ | -------- | ----------------------------------------------------------------- |
| settings  | object | Required | An object containing one or more of the editable settings keys.   |

### HTTP Request

`PATCH https://www.kalendme.com/api/v1/organization`
