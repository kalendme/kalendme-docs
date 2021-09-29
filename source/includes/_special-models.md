# Special Models

These are common special models across our API.

## Week Availability

```json
{
  "weekAvailability": {
    "0": [ { "start": "00:00", "end": "00:00" } ], // Sunday -- Not availabile 
    "1": [ { "start": "08:00", "end": "18:00" } ], // Monday
    "2": [ { "start": "08:00", "end": "18:00" } ], // Tuesday
    "3": [ { "start": "08:00", "end": "18:00" } ], // Wednesday
    "4": [ { "start": "08:00", "end": "18:00" } ], // Thursday
    "5": [ { "start": "08:00", "end": "18:00" } ], // Friday
    "6": [ { "start": "08:00", "end": "18:00" } ], // Saturday -- Not availabile 
  }
}
```

Used to describe a user's or an event's week availability. 0 through 6 represent the days of the week starting on sunday. All times are represented in local time to the user's configured time zone.

## Custom Question

```json
{
  "id": "3656475289d4aecd03804ec4d6045953", // An ID of your choosing to map to your system
  "order": 0,
  "question": "Whats your phone number?"
}
```

Used for specific user event types as questions for guests to fill out. They have an id, order in which to display to the user and the question itself.