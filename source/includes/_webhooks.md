# Webhooks

## Events

Events are our way of letting you know when something interesting happens in your account. When an interesting event occurs, we create a new `Event` object. For example, when a Context is added to a Session, we create a `context.session.context_added` event, and when a bundle subscription is canceled, we create an `context.subscription.canceled` event.

### The Event object

> Example webhook event

```
{
  "id": "evt_a75f6d23be8c17b1",
  "object": "event",
  "type": "context.session.context_added",
  "created": 1709679883853,
  "api_version": "2023-12-10",
  "request": {
    "id": "req_2442fd8627bedb77",
    "idempotency_key": null
  },
  "data": {
    "context_id": "6b0af623-2923-4997-92a6-73f94bbe321e",
    "client_id": "286d2a536a5d6b6b",
    "app_id": "417f3625d067cbe3",
    "created_at": "2024-03-05T23:04:43.684Z",
    "session_id": "4c682b9e-65b4-4e43-a5e9-96f58a967240"
  }
}
```
**Attributes**

**`id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the webhook event.

**`object`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Description of the object type (e.g., `event`).

**`type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Description of the event (e.g., `context.session.context_added` or `context.subscription.canceled`)

**`created`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
`UTC` timestamp when the event was created. 

**`api_version`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The Paket API version through which the triggering API request was made.

**`request`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
An object containing details of the event's triggering API request.

**`request.id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the event's triggering API request.

**`request.idempotency_key`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The `idempotency_key`, if any, of the original request. Otherwise `null`.

**`data`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
An object containing the data associated with the event.

### Types of events

This is a list of all the types of events we currently send. We may add more at any time, so in developing and maintaining your code, you should not assume that only these types exist.

You’ll notice that these events follow a pattern: `resource.event`. Our goal is to design a consistent system that makes things easier to anticipate and code against.

**Event**

**`context.session.deleted`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session context](#core-api-sessions-the-session-context-object)</span>  
Occurs when a Session is removed. Sent to the related Publishers only.

**`context.session.context_added`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session context](#core-api-sessions-the-session-context-object)</span>  
Occurs when a Context is added to a Session. Sent to the related Publishers only.

**`context.session.context_removed`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session context](#core-api-sessions-the-session-context-object)</span>  
Occurs when a Context is removed from a Session. Sent to the related Publishers only.

**`context.list.item_hidden`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [list item](#upnext-api-overview-the-list-item-object)</span>  
Occurs when a Context user removes or hides an item from their List via the Platform. Sent to the related Publisher only.