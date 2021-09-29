# Introduction

Welcome to the KalendMe's API! You can use our API endpoints to build your next platform and skip all scheduling headaches with a simple to use integration.

You can view code examples in the dark area to the right.

# Authentication

> To test authentication works, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://www.kalendme.com/api/v1/users" \
  -H "Authorization: Bearer abcdef123456"
```

> Make sure to replace `abcdef123456` with your API key.

KalendMe uses API keys to allow access to the API. You can get an API key by [creating a free account](https://www.kalendme.com/signin) and then heading over to settings to create an Organization. 

<aside class="warning">You will only be able to use our API with an Organization account.</aside>

KalendMe expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer abcdef123456`

<aside class="notice">
  You must replace <code>abcdef123456</code> with your own API key.
</aside>
