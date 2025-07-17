# UpNext API

## Overview

The Paket UpNext API is a powerful connected TV (CTV) engagement tool through which Platforms and Publishers can offer users a consistent, highly personalized feed of their recently watched items, new content availability, watchlist items, and more. 

While similar features currently exist across many best-in-class tv and OEM platforms, the resulting data is, invariably, inconsistent - and the means by which the data is retrieved are often varied, making it challenging for app developers to build and maintain such feature across an array of platforms. 

The goal with the Paket UpNext API is to offer CTV Platforms and Publishers a platform-agnostic experience that is as consistent and reliable - across devices - as is currently available exclusively on identity-based, ecosystem-wide platforms such as Google TV and Apple TV.

**For Platforms**, the UpNext API offers a timely snapshot of what a user is watching on any Paket-integrated service across any platorm or device. In addition to providing an aggregated view of a user's most relevant content, the data provided can be used to drive personalized recommendations, ads, and merchandising opportunities from within the Platform's native environment.

**For Publishers**, the UpNext API ensures its highly relevant content is featured consistently across Paket-integrated tv platforms and within the context of a user's cross-device viewing - something that has proven to be a top driver of engagement on connected tvs. Further, a Publisher needs to integrate only once to have its users' continue watching data syndicated to any Paket-integrated platform. Adding new Platforms is simply a matter of configuration from within the [Paket Developer Portal](https://developer.paket.tv).

### The List Object

At the core of the UpNext API is the Session List object, which is an aggregated, chronological feed up UpNext items added by all Contexts attached to a Session. 

For example, if Apps A, B, and C have shared their respective Context IDs with a Platform - and such Context IDs have been added to a Session - a request to [this API endpoint](#upnext-api-sessions-get-an-aggregated-list) will return an aggregated, chronological list of items across all Apps attached to the Session. 

An individual Context's feed (i.e., a specific App) can also be requested simply by calling the Session API along with the [requested Context ID](#upnext-api-sessions-get-a-specific-list) included in the path parameters.

<!-- ========================= -->
<!-- UPNEXT SESSION ATTRIBUTES -->
<!-- ========================= -->

> The List Object

```
{
  "total": 271,
  "next_key": 4,
  "items": [
    {...},
    {...},
    {...},
    {...},
    }
  ]
}
```
**Attributes**

**`total`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
The total number of list items returned from request.

**`next_key`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
The index of the next available item. Returned only if the `total` results exceeds the requested number of results.

**`items`** <span style='margin: 0 5px;font-size:.9em'>array</span>  
An array of [list items](#upnext-api-overview-the-list-item-object).

### The List Item Object

> The List Item Object

```
{
  "content_id": "1iwousk12i008d9",
  "app_id": "AP463772054442217472",
  "context_id": "CX467798389036421120",
  "updated_at": "2023-12-20T19:02:28.285Z",
  "action": "up_next_continue",
  "media_type": "tv_episode",
  "title": "The Colt",
  "description": "Jeff ignores Lassie when Gramps brings home a new colt.",
  "season": 1,
  "episode": 3,
  "poster_art_uri": "https://media.paket.tv/1iwousk12i008d9/thumb.png",
  "poster_art_aspect_ratio": "aspect_ratio_16_9",
  "preview_video_uri": "https://media.paket.tv/1iwousk12i008d9/preview.m3u8",
  "position": 654,
  "duration": 1308,
  "hidden": false,
  "platform_data": {
    "platform_app_id": "internalAppId12345"
  },
  "app_media": {
      "icon_1x": "https://media.paket.tv/media/df79418d4e6d5325/icon@1x.png",
      "icon_2x": "https://media.paket.tv/media/df79418d4e6d5325/icon@2x.png",
      "icon_3x": "https://media.paket.tv/media/df79418d4e6d5325/icon@3x.png",
      "tile_1x": "https://media.paket.tv/media/df79418d4e6d5325/tile@1x.png",
      "tile_2x": "https://media.paket.tv/media/df79418d4e6d5325/tile@2x.png",
      "logo_dark_1x": "https://media.paket.tv/media/df79418d4e6d5325/logo_dark@1x.png",
      "logo_light_1x": "https://media.paket.tv/media/df79418d4e6d5325/logo_light@1x.png"
    }
}
```

**Attributes**

**`content_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the respective item. Typically, this is an internal identifier provided by the Publisher.

**`app_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The App ID to which the item is associated.

**`context_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Context ID to which the item is associated.

**`updated_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The ISO 8601 timestamp when the item was created or last updated.

**`action`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The item action, which can be one of the following enum values:

- `up_next_continue`
- `up_next_next`
- `up_next_new`
- `up_next_watchlist`

**`media_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The item's media type, which can be one of the following enum values:

- `movie`
- `tv_episode`

**`title`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The title of the movie or TV show item.

**`description`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The item description.

**`season`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
The tv series season number. Returned only if item `media_type` is `tv_episode`.

**`episode`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
The tv series episode number. Returned only if item `media_type` is `tv_episode`.

**`poster_art_uri`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Uri of item's thumbnail image.

**`poster_art_aspect_ratio`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Aspect ratio of media provided at `poster_art_uri`, which can be one of the following enum values:

- `aspect_ratio_16_9`
- `aspect_ratio_4_3`
- `aspect_ratio_1_1`
- `aspect_ratio_2_3`

**`preview_video_uri`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Uri of item's preview video, if provided.

**`position`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Last recorded position of media playback in seconds (returned if `action` is `up_next_continue`).

**`duration`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Item's total running time in seconds.

**`hidden`** <span style='margin: 0 5px;font-size:.9em'>boolean</span>  
Indicates whether item is hidden or visible.

**`platform_data`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Custom object containing [custom attributes](#platform-data) provided by the requesting Platform in the form of key-value pairs.

**`app_media`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Object containing URIs to app media provided by the integrated Publisher.

Media URIs are constructed using the following shchema: `<base_url>/media/<app_id>/<file_name>.png`. Media will return provided the respective file has been uploaded by the Publisher. Note: this can be reviewed and verified by the Platform via the UpNext API avails configuration in the [Paket Developer Portal](https://developer.paket.tv).

Below is a list of available keys and its respective `file_name`:

- `icon_1x` corresponds to `icon@1x.png` (240x240, PNG)
- `icon_2x` corresponds to `icon@2x.png` (480x480, PNG)
- `icon_3x` corresponds to `icon@3x.png` (960x960, PNG)
- `tile_1x` corresponds to `tile@1x.png` (400x240, PNG)
- `tile_2x` corresponds to `tile@2x.png` (800x480, PNG)
- `logo_dark_1x` corresponds to `logo_dark@1x.png` (400x240, PNG, Transparency)
- `logo_light_1x` corresponds to `logo_light@1x.png` (400x240, PNG, Transpareny)

<aside class="notice">
The <span style="font-family:monospace;font-size:.9em;font-weight:bolder">platform_data</span> and <span style="font-family:monospace;font-size:.9em;font-weight:bolder">app_media</span> objects are returned only via the Sessions endpoint.
</aside>


### The Session Context Object

The Session Context Object is created when a Participant's `context_id` is added to a Session or when requesting a list of all Parcticipants associated with an active Session. It contains useful information related to a Participant's relationship to a Session.


```
{
  "session_id: "SN759077888463135026"
  "context_id": "CX463135026759077888",
  "app_id": "AP463772054442217472",
  "created_at": "2024-02-16T04:41:06.596Z"
}
```
**Attributes**

**`app_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Publisher's App ID to which the `context_id` belongs.

**`context_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The app user's (or "Participant") unique Context identifier.

**`session_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform user's unique Session identifier.

**`created_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The ISO 8601 timestamp when the Session was created.


### Platform Work Flow

Before beginning, Platform partners should familiarize themselves with the [Sessions](#sessions) object. Additionally, Platforms should leverage the `platform_data` configuration as described in the [Platform Data](#platform-data) section of the documentation.

The diagram below is an overview of how Platform Clients interact with the Paket UpNext API. 

<aside class="notice">Please note that the Platform must provide a means for a Publisher's app to communicate the Paket Context ID to the Platform Client. Typically, this is achieved via the Platform SDK.</aside>

![Platform Work Flow Diagram](paket_upnext_workflow_platforms.jpg)

### Publisher Work Flow

Before beginning, Publisher partners should familiarize themselves with the [Contexts](#contexts) object.

The diagram below is an overview of how Publisher Clients interact with the Paket UpNext API. 

<aside class="notice">Please note that the Publisher must be able to communicate the Paket Context ID to the Platform Client. Typically, this is achieved via the Platform SDK.</aside>

![Publisher Work Flow Diagram](paket_upnext_workflow_publishers.jpg)


## Sessions

Sessions are the primary means by which Platforms interact with the Paket UpNext API. 

It is through this API that Platforms can: 

- Add or remove Contexts to a Session
- Fetch an aggregated UpNext list 
- Fetch a specific Context's UpNext list
- Fetch a list of a Session's Contexts
- Hide items from a list
- Remove an existing Session

### Add a Context

> Endpoint

```
PUT https://api.paket.tv/v1/sessions/upnext/:session_id
```

Adds or updates a Context to an active Session.


> PUT https://api.paket.tv/v1/sessions/upnext/:session_id

```curl
curl --location --request PUT 'https://api.paket.tv/v1/sessions/upnext/:session_id' \
  --header 'Content-Type: application/json' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>' \
  --data '{
    "app_id": "fe4a9ed381e8e376",
    "context_id": "6e5060c3-7df4-488e-9e9a-67cd5077b352"
  }'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "session_id: "SN759077888463135026"
  "context_id": "CX463135026759077888",
  "app_id": "AP463772054442217472",
  "created_at": "2024-02-16T04:41:06.596Z"
}

```

**Path Parameters**

\* required

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform user's unique Session identifier.

**Request Body**

**`app_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Publisher's App ID to which the Context being added belongs.

**`context_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Context ID to be added to the Session.

**Returns**

Returns a [Session Context Object](#upnext-api-overview-the-session-context-object).


### Remove a Context

> Endpoint

```
DELETE https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id
```

Removes a Context from an active Session.


> DELETE https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id

```curl
curl --location --request DELETE 'https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 204 OK
Content-Type: application/json
```

**Path Parameters**

\* required

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform user's unique Session identifier.

**`context_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The app user's (or "Participant") unique Context identifier.

**Returns**

Returns a `204` No Content response.

### Get an Aggregated List

> Endpoint

```
GET https://api.paket.tv/v1/sessions/upnext/:session_id
```

Retrieves aggregated Session list from all registered Contexts.


> GET https://api.paket.tv/v1/sessions/upnext/:session_id

```curl
curl --location 'https://api.paket.tv/v1/sessions/upnext/:session_id' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "total": 271,
  "next_key": 4,
  "items": [
    {
      "content_id": "1iwousk12i008d9",
      "app_id": "df79418d4e6d5325",
      "context_id": "6b0af623-2923-4997-92a6-73f94bbe321e",
      "updated_at": "2023-12-20T19:02:28.285Z",
      "action": "up_next_continue",
      "media_type": "tv_episode",
      "title": "The Colt",
      "description": "Jeff ignores Lassie when Gramps brings home a new colt.",
      "season": 1,
      "episode": 3,
      "poster_art_uri": "https://media.paket.tv/1iwousk12i008d9/thumb.png",
      "poster_art_aspect_ratio": "aspect_ratio_16_9",
      "preview_video_uri": "https://media.paket.tv/1iwousk12i008d9/preview.m3u8",
      "position": 654,
      "duration": 1308,
      "hidden": false,
      "platform_data": {
        "platform_app_id": "internalAppId12345"
      },
      "app_media": {
        "icon_1x": "https://media.paket.tv/media/df79418d4e6d5325/icon@1x.png",
        "icon_2x": "https://media.paket.tv/media/df79418d4e6d5325/icon@2x.png",
        "icon_3x": "https://media.paket.tv/media/df79418d4e6d5325/icon@3x.png",
        "tile_1x": "https://media.paket.tv/media/df79418d4e6d5325/tile@1x.png",
        "tile_2x": "https://media.paket.tv/media/df79418d4e6d5325/tile@2x.png",
        "logo_dark_1x": "https://media.paket.tv/media/df79418d4e6d5325/logo_dark@1x.png",
        "logo_light_1x": "https://media.paket.tv/media/df79418d4e6d5325/logo_light@1x.png"
      }
    },
    {...},
    {...},
    {...}
  ]
}
```

**Path Parameters**

\* required

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform user's unique Session identifier.

**Query Parameters**

**`limit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Limits items returned (default is `50`, maximum is `200`).

**`next_key`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Key returned in previous call if [additional items](#pagination) are available.

**Returns**

Returns a [List Object](#upnext-api-overview-the-list-object) containing an array of [List Item Objects](#upnext-api-overview-the-list-item-object).


### Get a Specific List

> Endpoint

```
GET https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id
```

Retrieves the Session list of a specified Context.


> GET https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id

```curl
curl --location 'https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "total": 271,
  "next_key": 4,
  "items": [
    {
      "content_id": "1iwousk12i008d9",
      "app_id": "df79418d4e6d5325",
      "context_id": "6b0af623-2923-4997-92a6-73f94bbe321e",
      "updated_at": "2023-12-20T19:02:28.285Z",
      "action": "up_next_continue",
      "media_type": "tv_episode",
      "title": "The Colt",
      "description": "Jeff ignores Lassie when Gramps brings home a new colt.",
      "season": 1,
      "episode": 3,
      "poster_art_uri": "https://media.paket.tv/1iwousk12i008d9/thumb.png",
      "poster_art_aspect_ratio": "aspect_ratio_16_9",
      "preview_video_uri": "https://media.paket.tv/1iwousk12i008d9/preview.m3u8",
      "position": 654,
      "duration": 1308,
      "hidden": false,
      "platform_data": {
        "platform_app_id": "internalAppId12345"
      },
      "app_media": {
        "icon_1x": "https://media.paket.tv/media/df79418d4e6d5325/icon@1x.png",
        "icon_2x": "https://media.paket.tv/media/df79418d4e6d5325/icon@2x.png",
        "icon_3x": "https://media.paket.tv/media/df79418d4e6d5325/icon@3x.png",
        "tile_1x": "https://media.paket.tv/media/df79418d4e6d5325/tile@1x.png",
        "tile_2x": "https://media.paket.tv/media/df79418d4e6d5325/tile@2x.png",
        "logo_dark_1x": "https://media.paket.tv/media/df79418d4e6d5325/logo_dark@1x.png",
        "logo_light_1x": "https://media.paket.tv/media/df79418d4e6d5325/logo_light@1x.png"
      }
    },
    {...},
    {...},
    {...}
  ]
}
```

**Path Parameters**

\* required

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform user's unique Session identifier.

**`context_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The app user's (or "Participant") unique Context identifier.

**Query Parameters**

**`limit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Limits items returned (default is `50`, maximum is `200`).

**`next_key`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Key returned in previous call if [additional items](#pagination) are available.

**Returns**

Returns a [List Object](#upnext-api-overview-the-list-object) containing an array of [List Item Objects](#upnext-api-overview-the-list-item-object).

### Get Session Contexts

> Endpoint

```
GET https://api.paket.tv/v1/sessions/upnext/:session_id/contexts
```

Retrieves a list of all Context's associated with a Session.


> GET https://api.paket.tv/v1/sessions/upnext/:session_id/contexts

```curl
curl --location 'https://api.paket.tv/v1/sessions/upnext/:session_id/contexts' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "total": 12,
  "next_key": 4,
  "items": [
    {
      {
        "session_id: "SN759077888463135026",
        "context_id": "6b0af623-2923-4997-92a6-73f94bbe321e",
        "app_id": "df79418d4e6d5325",
        "created_at": "2023-12-23T23:38:50.303Z"
      }
    },
    {...},
    {...},
    {...}
  ]
}
```

**Path Parameters**

\* required

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform user's unique Session identifier.


**Query Parameters**

**`limit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Limits items returned (default and maximum is `50`).

**`next_key`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Key returned in previous call if [additional items](#pagination) are available.

**Returns**

Returns a [List Object](#upnext-api-overview-the-list-object) containing an array of [Session Contexts Objects](#upnext-api-overview-the-session-contexts-object).

### Hide an Item in a List

> Endpoint

```
PUT https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id
```

> PUT https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id

```curl
curl --location --request PUT 'https://api.paket.tv/v1/sessions/upnext/:session_id/:context_id' \
  --header 'Content-Type: application/json' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>' \
  --data '{
    "content_id": "1iwousk12i008d9",
    "action": "hide"
  }'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "message": "Success"
}
```

If hiding list items is supported within a Platform's user interface, this action allows the Platform to set the `hidden` parameter of a list item to `true`. Once the action is completed, an `item_hidden` notification is delivered to the Publisher's webhook endpoint.

While it is up to the Publisher to decide whether or not to remove the item from the Context's list, the item will remain hidden within the [List Item Object](#upnext-api-overview-the-list-item-object) unless the `hidden` attribute is set to `false` by a Publisher action.

**Path Parameters**

\* required

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform user's unique Session identifier.

**`context_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The app user's (or "Participant") unique Context identifier.

**Request Body**

**`content_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The content ID of the item to be hidden.

**`action`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The hide action, which can be one of the following enum values: 

- `hide`
- `unhide`

**Returns**

Returns a `200` Success response.

### Remove a Session

> Endpoint

```
DELETE https://api.paket.tv/v1/sessions/upnext/:session_id
```

> DELETE https://api.paket.tv/v1/sessions/upnext/:session_id

```curl
curl --location --request DELETE 'https://api.paket.tv/v1/sessions/upnext/:session_id' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 204 OK
Content-Type: application/json
```

Deletes a Session and all Context associations.

**Path Parameters**

\* required

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform user's unique Session identifier.

**Returns**

Returns a `204` No Content response.

## Contexts

Contexts are the primary means by which Publishers interact with the Paket UpNext API. 

It is through this API that Publishers can: 

- Add or remove items from a Context's list
- Fetch a specific Context's UpNext list
- Remove a Context's Sessions

### Publisher Guidelines

It is likely that most Publishers who will be integrating with the Paket UpNext API will have an existing back-end service dedicated to making continue watching, new episodes, and related requests. In such cases - and to ensure as consistent a user experience as possible - it is recommended that such partners post data to the UpNext API in a way that is consistent with its existing guidelines and best practices.

For those Publishers who will be integrating exclusively with the Paket API, please refer to the following guidelines as to when items should be added to a Context's UpNext List.

**When to add an item to the UpNext List**

Please determine whether or not to add an `up_next_continue` item if:

- The user exits an app client during playback
- The user pauses or stops playback of a media item for more than five (5) minutes

**Including unfinished movies and TV shows**

Please add an `up_next_continue` item if:

- A movie or TV show is started and the user has watched more than two (2) minutes; or
- A movie or TV show is is started and the user has watched more that 3% of its total duration (whichever is earlier)

**Including new and next episode from a tv series**

- Please add an `up_next_next` item if a user finishes an episode in a series and the next episode is available on your service
- Please add an `up_next_new` item if a user is caught up on all available episodes of a tv series and a new season or episode becomes availables
- Please add an `up_next_watchlist` item if an item in a user's watchlist was previously unavailable but has since been made available to watch on the service 

**Remove finished items from the UpNext List**

- A media item is considered 'finished' when the end credits begin
- If the end credits cannot be determined, a media item is considered 'finished' when 3% or less the total duration of the media is remaining to be watched

### Add Item to List

> Endpoint

```
PUT https://api.paket.tv/v1/contexts/upnext/:context_id
```

> PUT https://api.paket.tv/v1/contexts/upnext/:context_id

```curl
curl --location --request PUT 'https://api.paket.tv/v1/contexts/upnext/:context_id' \
  --header 'Content-Type: application/json' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>' \
  --data '{
    "action": "up_next_continue",
    "media_type": "tv_episode",
    "content_id": "1iwousk12i008d9",
    "title": "The Colt",
    "poster_art_uri": "https://media.paket.tv/1iwousk12i008d9/thumb.png",
    "poster_art_aspect_ratio": "aspect_ratio_16_9",
    "description": "Jeff ignores Lassie when Gramps brings home a new colt.",
    "season": 1,
    "episode": 3,
    "preview_video_uri": "https://media.paket.tv/1iwousk12i008d9/preview.m3u8",
    "position": 654,
    "duration": 1308
    "hidden": false
  }'
``` 

> Response

```http
HTTP/1.1 204 OK
Content-Type: application/json
{
  "message": "Success"
}
```

Adds or updates an Item to the Context list. Please refer to the UpNext [Guidelines](#upnext-api-contexts-guidelines) to learn more about how to determine when to add an Item to the Context's list.

**Path Parameters**

\* required

**`context_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Context ID to which the List belongs.

**Request Body**

\* required

**`content_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The content ID of the item being added. Typically, this is an internal identifier provided by the Publisher.

**`media_type`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The item's media type, which can be one of the following enum values:

- `movie`
- `tv_episode`

**`action`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The item action, which can be one of the following enum values:

- `up_next_continue`
- `up_next_next`
- `up_next_new`
- `up_next_watchlist`

**`title`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The title of the movie or TV show item.

**`description`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The item description.

**`poster_art_uri`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Uri of item's thumbnail image.

**`poster_art_aspect_ratio`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Aspect ratio of media provided at `poster_art_uri`, which can be one of the following enum values:

- `aspect_ratio_16_9`
- `aspect_ratio_4_3`
- `aspect_ratio_1_1`
- `aspect_ratio_2_3`

**`season`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
The tv series season number. Required only if item `media_type` is `tv_episode`.

**`episode`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
The tv series episode number. Required only if item `media_type` is `tv_episode`.

**`preview_video_uri`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Uri of item's preview video, if provided.

**`position`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Last recorded position of media playback in seconds (required if `action` is `up_next_continue`).

**`duration`*** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Item's total running time in seconds.

**`hidden`** <span style='margin: 0 5px;font-size:.9em'>boolean</span>  
Indicates whether item is hidden or visible.

**Returns**

Returns a `200` Success response.

### Remove Item from List

> Endpoint

```
DELETE https://api.paket.tv/v1/contexts/upnext/:context_id/:content_id
```

> DELETE https://api.paket.tv/v1/contexts/upnext/:context_id/:content_id

```curl
curl --location --request DELETE 'https://api.paket.tv/v1/contexts/upnext/:context_id/:content_id' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 204 OK
Content-Type: application/json
```

Removes an Item from the Context's list.

**Path Parameters**

\* required

**`context_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Context ID to which the List belongs.

**`content_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Content ID of the item being added. Typically, this is an internal identifier provided by the Publisher.

**Returns**

Returns a `204` No Content response.

### Get a Context's List

> Endpoint

```
GET https://api.paket.tv/v1/contexts/upnext/:context_id
```

Retrieves the list of a specified Context.


> GET https://api.paket.tv/v1/contexts/upnext/:context_id

```curl
curl --location 'https://api.paket.tv/v1/contexts/upnext/:context_id' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "total": 271,
  "next_key": 4,
  "items": [
    {
      "content_id": "1iwousk12i008d9",
      "app_id": "df79418d4e6d5325",
      "context_id": "6b0af623-2923-4997-92a6-73f94bbe321e",
      "updated_at": "2023-12-20T19:02:28.285Z",
      "action": "up_next_continue",
      "media_type": "tv_episode",
      "title": "The Colt",
      "description": "Jeff ignores Lassie when Gramps brings home a new colt.",
      "season": 1,
      "episode": 3,
      "poster_art_uri": "https://media.paket.tv/1iwousk12i008d9/thumb.png",
      "poster_art_aspect_ratio": "aspect_ratio_16_9",
      "preview_video_uri": "https://media.paket.tv/1iwousk12i008d9/preview.m3u8",
      "position": 654,
      "duration": 1308,
      "hidden": false
    },
    {...},
    {...},
    {...}
  ]
}
```


**Path Parameters**

\* required

**`context_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Context ID to which the List belongs.

**Query Parameters**

**`limit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Limits items returned (default is `50`, maximum is `200`).

**`next_key`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Key returned in previous call if [additional items](#pagination) are available.

**Returns**

Returns a [List Object](#upnext-api-overview-the-list-object) containing an array of [List Item Objects](#upnext-api-overview-the-list-item-object).

<aside class="notice">
The <span style="font-family:monospace;font-size:.9em;font-weight:bolder">platform_data</span> object is returned only via the Sessions endpoint and will not be included in the response to this request.
</aside>

### Remove Context's Sessions

> Endpoint

```
DELETE https://api-dev.paket.tv/v1/contexts/upnext/:context_id/sessions
```

Removes all sessions associated with a Context.


> DELETE https://api-dev.paket.tv/v1/contexts/upnext/:context_id/sessions

```curl
curl --location --request DELETE 'https://api.paket.tv/v1/contexts/upnext/:context_id/sessions' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 204 OK
Content-Type: application/json
```

**Path Parameters**

\* required

**`context_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Context ID to which the Sessions are associated.

**Returns**

Returns a `204` No Content response.