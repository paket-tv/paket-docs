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
    "client_id: "fab3d02606122a08",
    "app_id: "AP467777522151985152"
}
```

**Attributes**

**`message`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The API Status message.

**`client_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the requesting Client.

**`app_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the requesting Client's App (returned if requesting tenant is an App).

**`platform_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the requesting Client's App (returned if requesting tenant is a Platform).

### Retrieve API Status

> GET https://api.paket.tv/v1

```curl
curl --location 'https://api.paket.tv/v1' \
    --header 'Accept: application/json' \
    --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
    "message": "The API is healthy!",
    "client_id: "fab3d02606122a08",
    "app_id: "AP467777522151985152"
}
```

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
  "session_id": "759077888463135026",
  "client_id": "907785425416524528",
  "platform_id": "PL442217472463772054",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Attributes**

**`session_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
A unique Session identifier.

**`client_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The requesting Client ID.

**`platform_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Platform to which the requesting Client belongs.

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

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "session_id": "759077888463135026",
  "client_id": "907785425416524528",
  "platform_id": "PL442217472463772054",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Returns**

Returns a status object.


## Contexts

> Endpoint

```
POST https://api.paket.tv/v1/contexts
```

A Context is a unique identifier representing a Publisher or app's end user either at the account or profile level. Also referred to as a "Participant," whether it is associated to the account or profile is, ultimately, up to the Publisher to determine. For optimal flexibility, however, a Context should be created and stored by a Publisher at the profile level.

Ideally, a Context should be indefinitely associated with an end-user's account or profile. So long as the Context ID persists, all items associated with the Context will remain active.

Generally, a Publisher interacts with a Platform by explicitly sharing the Context ID with a Platform and it being matched to an active Session.

### The Context Object

> The Context Object

```
{
  "context_id": "CX467798389036421120",
  "client_id": "f02d9e4750febfa4",
  "app_id": "AP463772054442217472",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Attributes**

**`context_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The newly generated Context ID.

**`client_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The requesting Client ID.

**`app_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The App to which the requesting Client belongs.

**`created_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The ISO 8601 timestamp when the Session was created.

### Create a new Context

> POST https://api.paket.tv/v1/contexts

```curl
curl --location --request POST 'https://api.paket.tv/v1/contexts' \
    --header 'Accept: application/json' \
    --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "context_id": "CX467798389036421120",
  "client_id": "f02d9e4750febfa4",
  "app_id": "AP463772054442217472",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```

**Parameters**

No parameters.

**Returns**

Returns a Context object.