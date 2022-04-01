# Time Zones

## Overview

Time zones are a critical component for scheduling. KalendMe provides this endpoint to be able to always be up to date with the latest IANA time zone DB.

Current version: 2022a

## Get Time Zones

```shell
curl "https://www.kalendme.com/api/v1/time-zones" \
  -H "Authorization: Bearer abcdef123456"
```

> The above command returns JSON structured like this:

```json
{
  "timeZones": [
		"Africa/Algiers",
		"Atlantic/Cape_Verde",
		"Africa/Ndjamena",
		"Africa/Abidjan",
		"Africa/Accra",
    ....
  ]
}

```
