# Webhooks

## Events

Events are our way of letting you know when something interesting happens in your account. When an interesting event occurs, we create a new `Event` object. For example, when a Context is added to a Session, we create a `context.session.context_added` event, and when a bundle subscription is canceled, we create an `context.subscription.canceled` event.

### The Event object

> Example webhook event

```json
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

**Events**

**`context.session.deleted`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session context](#core-api-sessions-the-session-context-object)</span>  
Occurs when a Session is removed.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) that have contexts in the deleted session</span>

**`context.session.context_added`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session context](#core-api-sessions-the-session-context-object)</span>  
Occurs when a Context is added to a Session.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) that own the added context</span>

**`context.session.context_removed`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [session context](#core-api-sessions-the-session-context-object)</span>  
Occurs when a Context is removed from a Session.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) that own the removed context</span>

**`context.list.item_hidden`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [list item](#upnext-api-overview-the-list-item-object)</span>  
Occurs when a Context user removes or hides an item from their List via the Platform.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) that own the hidden item</span>

**`subscription.status.created`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [subscription](#license-api-subscriptions-the-subscription-object)</span>  
Occurs when a platform creates a subscription that includes a publisher's product.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products are included in the subscription</span>

**`subscription.status.renewed`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [subscription](#license-api-subscriptions-the-subscription-object)</span>  
Occurs when a platform successfully renews a subscription that includes a publisher's product.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products are included in the subscription</span>

**`subscription.status.cancels_on`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [subscription](#license-api-subscriptions-the-subscription-object)</span>  
Occurs when a subscription is scheduled to cancel at the end of the current period.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products are included in the subscription</span>

**`subscription.status.canceled`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [subscription](#license-api-subscriptions-the-subscription-object)</span>  
Occurs when a subscription is actually canceled (immediately or at period end) by platform or due to non payment.
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products were included in the canceled subscription</span>

**`subscription.status.paused`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [subscription](#license-api-subscriptions-the-subscription-object)</span>  
Occurs when a platform pauses a subscription.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products are included in the paused subscription</span>

**`subscription.status.resumed`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [subscription](#license-api-subscriptions-the-subscription-object)</span>  
Occurs when a platform resumes a paused subscription or reactivates a subscription scheduled for cancellation.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products are included in the resumed subscription</span>

**`subscription.invoice.created`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is an [invoice](#license-api-invoices-the-invoice-object)</span>  
Occurs when a new invoice is generated for a subscription (renewal or initial billing).  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Platforms that created the subscription receiving the invoice</span>

**`subscription.invoice.past_due`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is a [subscription](#license-api-subscriptions-the-subscription-object)</span>  
Occurs when subscription enters grace period after non payment or payment failure.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products are included in the subscription entering grace period</span>

**`activation.session.created`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is an [activation session](#license-api-activation-activation-session-object)</span>  
Occurs when payment succeeds and activation session is created.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products require activation in the session</span>

**`activation.code.reissued`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is activation exchange details</span>  
Occurs when a platform regenerates or reissues a new activation code.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products require activation in the session</span>

**`activation.code.expired`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is activation exchange details</span>  
Occurs when an activation code expires or is invalidated. 
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Publishers (Apps) whose products require activation in the session</span>

**`activation.item.failed`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is activation item details</span>  
Occurs when an activation attempt fails (before code expiration).  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Platforms that created the subscription containing the failed activation item</span>

**`activation.item.completed`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is activation item details</span>  
Occurs when a publisher confirms successful activation via the Update Activation Status endpoint.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Platforms that created the subscription containing the activated item</span>

**`activation.session.completed`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is an [activation session](#license-api-activation-activation-session-object)</span>  
Occurs when all items in an activation session are activated.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Platforms that created the subscription with the completed activation session</span>

**`activation.session.expired`** <span style='margin: 0 5px;font-size:.8em'>`data.object` is an [activation session](#license-api-activation-activation-session-object)</span>  
Occurs when activation session expires after 7 days without completion.  
<span style='margin: 0 5px;font-size:.8em;color:#666'>**Recipients**: Platforms that created the subscription with the expired activation session</span>