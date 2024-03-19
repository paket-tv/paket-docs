# Core API

The Core API includes requests that are used for more than one API product and, in many instances, across all of Paket's API products.

## API Status

> Endpoint

```
GET https://api.paket.tv/v1
```

Retrieves the API status by making an authenticated `GET` request to the API's root path.

### The Status Object

> The Status Object

```
{
    "message": "The API is healthy!",
    "client_id: "fab3d02606122a08"
}
```

**Attributes**

**`message`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The API Status message.

**`client_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the requesting Client.

### Retrieve API Status

> GET https://api.paket.tv/v1

```curl
curl --location 'https://api.paket.tv/v1' \
    --header 'Accept: application/json' \
    --header 'Authorization: Basic <credentials>'
``` 

> Response

```
{
    "message": "The API is healthy!"
}
```

**Parameters**

No parameters.

**Returns**

Returns a status object.

## Sessions

> Endpoint

```
POST https://api.paket.tv/v1/sessions
```

A Session represents a Platform end user either at the account or profile level. Whether the Session is associated to the account or profile is, ultimately, up to the Platform to determine. For optimal flexibility, however, a Session ID should be created and stored by a Platform at the profile level.

Ideally, a Session should be indefinitely associated with an end-user's account or profile. So long as the Session ID persists, all items associated with the Session will remain active.

### The Session Object

> The Session Object

```
{
  "session_id": "faa37296-dc32-4cce-b8c0-47ac4bc032c1",
  "client_type": "platform",
  "client_id": "fab3d02606122a08",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Attributes**

**`session_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
A unique Session identifier.

**`client_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The requesting Client type.

**`client_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The requesting Client ID.

**`created_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The ISO 8601 timestamp when the Session was created.

### The Session Participant Object

The Session Participant Object is created when a Participant is added to a Session and contains useful information related to a Participant's relationship to a Session.

> The Session Participant Object

```
{
  "app_id": "417f3625d067cbe3",
  "participant_id": "6b0af623-2923-4997-92a6-73f94bbe321e",
  "session_id": "faa37296-dc32-4cce-b8c0-47ac4bc032c1",
  "client_id": "fab3d02606122a08",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Attributes**

**`app_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Publisher's Client ID to which the `participant_id` belongs.

**`participant_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
A unique Participant identifier.

**`session_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
A unique Session identifier.

**`client_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The requesting Client type.

**`client_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The requesting Client ID.

**`created_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The ISO 8601 timestamp when the Session was created.

### Create a new Session

> POST https://api.paket.tv/v1/sessions

```curl
curl --location --request POST 'https://api.paket.tv/v1/sessions' \
    --header 'Accept: application/json' \
    --header 'Authorization: Basic <credentials>'
``` 

> Response

```
{
  "session_id": "faa37296-dc32-4cce-b8c0-47ac4bc032c1",
  "client_type": "platform",
  "client_id": "fab3d02606122a08",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Parameters**

No parameters.

**Returns**

Returns a Session object.

## Participants

> Endpoint

```
POST https://api.paket.tv/v1/participants
```

A Participant represents a Publisher (or app's) end user either at the account or profile level. Whether the Participant is associated to the account or profile is, ultimately, up to the Publisher to determine. For optimal flexibility, however, a Participant ID should be created and stored by a Publisher at the profile level.

Ideally, a Participant should be indefinitely associated with an end-user's account or profile. So long as the Participant ID persists, all items associated with the Participant will remain active.

Generally, a Publisher interacts with a Platform by explicitly sharing the Participant ID with a Platform and it being matched to an active Session.

### The Participant Object

> The Participant Object

```
{
  "participant_id": "6e5060c3-7df4-488e-9e9a-67cd5077b352",
  "client_type": "publisher",
  "client_id": "fe4a9ed381e8e376",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Attributes**

**`participant_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The newly generated Participant ID.

**`client_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The requesting Client type.

**`client_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The requesting Client ID.

**`created_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The ISO 8601 timestamp when the Session was created.

### Create a new Participant

> POST https://api.paket.tv/v1/participants

```curl
curl --location --request POST 'https://api.paket.tv/v1/participants' \
    --header 'Accept: application/json' \
    --header 'Authorization: Basic <credentials>'
``` 

> Response

```
{
  "participant_id": "6e5060c3-7df4-488e-9e9a-67cd5077b352",
  "client_type": "publisher",
  "client_id": "fe4a9ed381e8e376",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Parameters**

No parameters.

**Returns**

Returns a Participant object.