# Bento API

## Overview

The Paket Bento API is a license and subscription orchestration service that enables Platforms and Publishers to offer comprehensive subscription bundles through a unified catalog via their native billing and identity systems. This API provides access to curated plan offerings, subscription management, and invoice management capabilities that power the Paket subscription marketplace.

**For Platforms**, the Bundle API offers a seamless way to integrate third-party subscription services into their ecosystem, providing users with bundled offerings that combine multiple streaming services, apps, and digital content into single, manageable subscriptions.

**For Publishers**, the Bundle API ensures consistent subscription management across all Paket-integrated platforms, with unified billing, activation workflows, and revenue distribution handled automatically through the Paket infrastructure.

To get started, a Publisher must first configure their service via the Paket Publisher Portal. There, Publishers will be able to set up webhook endpoints, security, and API clients through which the service will communicate with the Paket service. It is in the Publisher Portal where Publishers will be able to set up product licenses and assign them for sale through approved Platform partners.

It is also in the Paket Publisher Portal where Platforms will configure API clients and configure plans and bundles for sale via the Paket API. 

### The Plan Object

At the core of the Bundle API is the Plan object, which defines subscription offerings with comprehensive pricing, billing cycles, and bundled services configuration.

> The Plan Object

```
{
  "plan_id": "427944e5ba9e",
  "name": "Disney+, Hulu, HBO Max Bundle",
  "plan_type": "sub_bundle",
  "status": "active",
  "platform_id": "PL468440696748511232",
  "free_trial_days": 0,
  "grace_period_days": 7,
  "billing_frequency": {
    "unit": "month",
    "value": 1
  },
  "media": {
    "promo_4k_1x": "https://media.paket.tv/media/plans/PL468440696748511232/427944e5ba9e/promo_4k@1x.png",
    "promo_2k_1x": "https://media.paket.tv/media/plans/PL468440696748511232/427944e5ba9e/promo_2k@1x.png"
  },
  "prices": {
    "US": [
      {
        "order": 1,
        "billing_cycles": 3,
        "price": {
          "price_in_cents": 1699,
          "tier_id": "1699",
          "currency_code": "USD",
          "price": 16.99
        }
      }
    ]
  },
  "localizations": {
    "en-us": {
        "description": "The bundle with everything you need",
        "display_name": "Disney+, Hulu, HBO Max Bundle"
    }
  },
  "plan_items": [
    {
        "status": "active",
        "price_wholesale": {
            "price": 4.56,
            "price_in_cents": 456,
            "currency_code": "USD"
        },
        "name": "Disney+ Basic",
        "app_id": "AP468442205989113856",
        "product_id": "PR469716925099413504",
        "localizations": {
            "en-us": {
                "description": "Disney+ Basic With Ads",
                "display_name": "Disney+ Basic"
            }
        },
        "prices": {
            "US": {
                "price_in_cents": 999,
                "tier_id": "999",
                "currency_code": "USD",
                "price": 9.99
            }
        },
        "app": {
            "id": "AP468442205989113856",
            "name": "Disney+",
            "media": {
                "icon_1x": "https://media.paket.tv/media/apps/AP468442205989113856/icon@1x.png",
                "icon_2x": "https://media.paket.tv/media/apps/AP468442205989113856/icon@2x.png",
                "icon_3x": "https://media.paket.tv/media/apps/AP468442205989113856/icon@3x.png",
                "logo_dark_1x": "https://media.paket.tv/media/apps/AP468442205989113856/logo_dark@1x.png",
                "logo_light_1x": "https://media.paket.tv/media/apps/AP468442205989113856/logo_light@1x.png",
                "tile_1x": "https://media.paket.tv/media/apps/AP468442205989113856/tile@1x.png",
                "tile_2x": "https://media.paket.tv/media/apps/AP468442205989113856/tile@2x.png"
            },
            "status": "live"
        }
    },
    ...
  ],
  "created_at": "2024-01-15T10:30:00.000Z",
  "updated_at": "2024-01-17T07:28:00.000Z"
}
```

**Attributes**

**`plan_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier for the plan.

**`name`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The internal name of the plan.

**`plan_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The type of plan, which can be one of the following enum values:

- `sub_bundle` - Multiple subscription services bundled together
- `sub_single` - Single subscription service

**`status`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The current status of the plan:

- `active` - Available for new subscriptions
- `inactive` - Not available for new subscriptions
- `deprecated` - Being phased out

**`platform_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform identifier associated with this plan.

**`free_trial_days`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Number of free trial days offered (0 if no trial).

**`grace_period_days`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Number of days after payment failure before subscription suspension.

**`billing_frequency`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
The billing cycle configuration with `unit` (month, year) and `value` (1, 3, 6, 12).

**`media`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
URLs to promotional media assets for the plan:

- `promo_4k_1x` - 4K promotional image
- `promo_2k_1x` - 2K promotional image

**`prices`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Regional pricing information organized by country code. Each region contains an array of pricing phases with:

- `order` - The sequence order of the pricing phase
- `billing_cycles` - Number of cycles this price applies to (omitted if null or indefinite)
- `price` - Price object containing:
  - `price_in_cents` - Price in cents as a integer
  - `tier_id` - Pricing tier identifier
  - `currency_code` - Three-letter ISO currency code
  - `price` - Price as a decimal

**`localizations`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Localized content organized by language code (e.g., "en-us"). Each localization contains:

- `description` - Localized description of the plan
- `display_name` - Localized display name for the plan

**`plan_items`** <span style='margin: 0 5px;font-size:.9em'>array</span>  
Array of products or entitlements included in the plan. Each item contains:

- `status` - Item status (active/inactive)
- `price_wholesale` - Wholesale pricing information with price, price_in_cents, and currency_code
- `name` - Internal name of the product
- `app_id` - Associated app identifier
- `product_id` - Unique product identifier
- `localizations` - Localized content for the product (description and display_name)
- `prices` - Regional retail pricing by country
- `app` - App object containing:
  - `id` - App identifier
  - `name` - App name
  - `media` - App media assets (icons, logos, tiles)
  - `status` - App status (live/inactive)

**`created_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp when the plan was created.

**`updated_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp when the plan was last updated.

### The Subscription Object

The Subscription object represents an active or inactive Subscription to a Plan, tracking billing cycles, payment status, and activation state.

> The Subscription Object

```
{
  "subscription_id": "SUB479027832035610624",
  "session_id": "SN468560588960960512",
  "plan_id": "427944e5ba9e",
  "plan_name": "Disney+, Hulu, HBO Max Bundle",
  "plan_type": "sub_bundle",
  "platform_id": "PL468440696748511232",
  "status": "active",
  "activation_status": "pending",
  "payment_status": "paid",
  "next_billing_date": "2025-09-14T20:45:35.064Z",
  "current_period_start": "2025-08-14T20:45:35.065Z",
  "current_period_end": "2025-09-14T20:45:35.064Z",
  "current_phase_id": "4282b4cfac23dc38",
  "billing_cycle_count": 1, 
  "free_trial_days": 0,
  "trial_end": null,
  "grace_period_days": 7,
  "grace_period_end": "2025-09-21T20:45:35.064Z",
  "billing_interval_days": 30,
  "billing_frequency": {
    "unit": "month",
    "value": 1
  },
  "tax_behavior": "exclusive",
  "tax_rate": 0.0875,
  "tax_type": "sales_tax",
  "tax_jurisdiction": "CA-Los Angeles",
  "tax_note": "",
  "payment_method_id": "pm_123",
  "activation_url": null,
  "activation_token": null,
  "metadata": {},
  "device_info": {},
  "replaced_subscription_id": null,
  "change_type": null,
  "proration_credit": 0,
  "cancel_at_period_end": false,
  "canceled_at": null,
  "ended_at": null,
  "created_ip": "75.85.168.125",
  "updated_ip": "75.85.168.125",
  "created_at": "2025-08-14T20:45:35.065Z",
  "updated_at": "2025-08-14T21:00:00.000Z"
}
```

**Attributes**

**`subscription_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier for the subscription (prefixed with SUB).

**`session_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The session identifier associated with this subscription.

**`plan_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The plan identifier this subscription is for.

**`plan_name`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The plan name.

**`plan_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The plan type, one of:

- `sub_bundle` - Plan with multiple associated products
- `sub_single` - Plan with a single associated product

**`platform_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform identifier issuing the plan.

**`status`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Current subscription status:

- `trialing` - In free trial period
- `active` - Active and paid
- `past_due` - Payment failed but in grace period
- `canceled` - Canceled by user
- `unpaid` - Payment required
- `paused` - Temporarily paused

**`activation_status`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Publisher activation status:

- `pending` - Awaiting activation
- `partial` - Partially activated (some services activated but not all)
- `completed` - Successfully activated
- `failed` - Activation failed

**`payment_status`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Current payment status:

- `paid` - Fully paid
- `unpaid` - Payment pending
- `scheduled` - Payment scheduled
- `no_payment_required` - Free or trial period
- `processing` - Payment in progress
- `failed` - Payment failed
- `canceled` - Payment canceled

**`next_billing_date`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp of the next billing date.

**`current_period_start`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp of the current billing period start.

**`current_period_end`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp of the current billing period end.

**`current_phase_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The current phase price identifier.

**`billing_cycle_count`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Total number of completed billing cycles.

**`free_trial_days`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Days included in free trial, if any.

**`trial_end`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp of the current trial period end.

**`grace_period_days`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Grace period during which subscription will remain active after failed payment.

**`grace_period_end`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp of the current grace period end.

**`billing_interval_days`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Billing interval in days.

**`billing_frequency`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Billing interval configuration.

- `unit` <span style='margin: 0 5px;font-size:.9em'>string</span> - Time unit. Values: `"month"`, `"day"`, `"year"`
- `value` <span style='margin: 0 5px;font-size:.9em'>integer</span> - Number of units between billing cycles

**`tax_behavior`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Specifies whether tax should be collected and how:

- `inclusive` - Tax will be included in plan price
- `exclusive` - Tax will be added to plan price
- `none` - Plan is not taxable

**`tax_rate`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Applied tax rate (0-1, e.g., 0.0875 for 8.75%).

**`tax_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Type of tax applied:

- `sales_tax`
- `vat`
- `gst`
- `pst`
- `hst`
- `none`

**`tax_jurisdiction`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Jurisdiction governing the applied tax.

**`tax_note`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Additional notes pertaining to the subscription tax.

**`payment_method_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Payment method identifier used for this subscription.

**`activation_url`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
URL provided by the publisher for activating bundled services.

**`activation_token`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Token used for activation process with the publisher.

**`metadata`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Additional custom metadata for the subscription.

**`device_info`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Additional device metadata for the subscription.

**`replaced_subscription_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Identifier of replaced subscription, if an upgrade or downgrade.

**`change_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Reason for change of subscription:

- `upgrade`
- `downgrade`
- `reactivation`

**`proration_credit`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Amount to credit or add to first invoice if mid-cycle upgrade or downgrade.

**`cancel_at_period_end`** <span style='margin: 0 5px;font-size:.9em'>boolean</span>  
Whether to cancel subscription at end of current period.

**`canceled_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp of date on which subscription was canceled.

**`ended_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp of the subscription end date.

**`created_ip`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The IP address from where the subscription was created.

**`updated_ip`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The IP address from where subscription was last updated.

**`created_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp when the subscription was created.

**`updated_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp when the subscription was last updated.

### The Invoice Object

The Invoice object represents a billing statement for a subscription period, tracking amounts, payment status, and retry logic.

> The Invoice Object

```
{
  "invoice_id": "INV479027832278880256",
  "invoice_number": "INV-2025-78880256",
  "subscription_id": "SUB479027832035610624",
  "platform_id": "PL468440696748511232",
  "invoice_date": "2025-08-14T20:45:35.122Z",
  "due_date": "2025-09-13T20:45:35.122Z",
  "status": "open",
  "payment_status": "unpaid",
  "subtotal": 1699,
  "proration_credit": 0,
  "tax_amount": 149,
  "total_amount": 1848,
  "amount_due": 1848,
  "amount_paid": 0,
  "currency": "USD",
  "tax_rate": 0.0875,
  "tax_type": "sales_tax",
  "tax_jurisdiction": "CA-Los Angeles",
  "tax_behavior": "exclusive",
  "tax_note": "",
  "payment_method_id": null,
  "payment_intent_id": null,
  "payment_date": null,
  "paid_at": null,
  "retry_count": 0,
  "max_retries": 3,
  "next_retry_date": null,
  "last_retry_date": null,
  "retry_delay_minutes": 60,
  "session_id": "session_456",
  "plan_id": "427944e5ba9e",
  "phase_id": "4282b4cfac23dc38",
  "phase_order": 1,
  "billing_cycle": 1,
  "period_start": "2025-08-14T20:45:35.065Z",
  "period_end": "2025-09-14T20:45:35.064Z",
  "region": "US",
  "plan_name": "Disney+, Hulu, HBO Max Bundle",
  "plan_type": "recurring",
  "platform_fee_rate": 0.03,
  "platform_fee_amount": 51,
  "metadata": {},
  "created_ip": "192.168.1.1",
  "updated_ip": "192.168.1.1",
  "created_at": "2025-08-14T20:45:35.122Z",
  "updated_at": "2025-08-14T20:45:35.122Z"
}
```

**Attributes**

**`invoice_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier for the invoice (prefixed with INV).

**`invoice_number`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Human-readable invoice number.

**`subscription_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The subscription this invoice belongs to.

**`status`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Invoice status:
- `open` - Awaiting payment
- `paid` - Fully paid
- `draft` - Not yet finalized

**`payment_status`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Payment collection status:
- `unpaid` - Not yet paid
- `paid` - Successfully paid
- `failed` - Payment failed
- `processing` - Payment in progress
- `canceled` - Payment canceled
- `scheduled` - Payment scheduled for future date
- `no_payment_required` - No payment needed (e.g., free trial)

**`subtotal`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Pre-tax amount in cents.

**`proration_credit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Proration credit applied to this invoice in cents (negative for credits).

**`tax_amount`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Tax amount in cents.

**`total_amount`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Total amount including tax in cents.

**`currency`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Three-letter ISO currency code.

**`tax_rate`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Applied tax rate (0-1, e.g., 0.0875 for 8.75%).

**`tax_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Type of tax applied:
- `sales_tax`
- `vat`
- `gst`
- `pst`
- `hst`
- `none`

**`tax_jurisdiction`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Tax jurisdiction code (e.g., "CA-Los Angeles").

**`tax_behavior`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Specifies whether tax should be collected and how:
- `inclusive` - Tax will be included in plan price
- `exclusive` - Tax will be added to plan price
- `none` - Plan is not taxable

**`tax_note`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Additional notes pertaining to the invoice tax.

**`payment_method_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Payment method used for this invoice.

**`payment_intent_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Payment processor intent identifier.

**`payment_date`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp when payment was processed (null if unpaid).

**`paid_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp when invoice was marked as paid (null if unpaid).

**`retry_count`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Number of payment retry attempts made.

**`max_retries`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Maximum number of payment retries allowed.

**`next_retry_date`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp for next payment retry attempt (null if no retry scheduled).

**`last_retry_date`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp of last payment retry attempt (null if no retries made).

**`retry_delay_minutes`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Delay in minutes before next retry attempt.

**`session_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The session identifier associated with this invoice's subscription.

**`plan_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The plan identifier for this invoice.

**`subscription_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The subscription this invoice belongs to.

**`phase_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The plan phase identifier used for pricing (without PHASE# prefix).

**`phase_order`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
The order number of the phase used for this invoice (1-based).

**`billing_cycle`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
The billing cycle number this invoice represents (0 for trial, 1+ for paid cycles).

**`period_start`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp for start of billing period.

**`period_end`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp for end of billing period.

**`region`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Region code for pricing (e.g., "US").

**`plan_name`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Display name of the plan.

**`plan_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Type of plan (e.g., "recurring", "one-time").

**`platform_fee_rate`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Platform fee rate (0-1, e.g., 0.03 for 3%).

**`platform_fee_amount`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Platform fee amount in cents.

**`metadata`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Additional metadata associated with the invoice.

**`platform_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The platform identifier that created this invoice's subscription.

**`created_ip`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
IP address from which the invoice was created.

**`updated_ip`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
IP address from which the invoice was last updated.

**`created_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp when the invoice was created.

**`updated_at`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
ISO 8601 timestamp when the invoice was last updated.

### Create Subscription Flow

Paket's subscription orchestration service works independently of the Platform's payment and identity services and the app Publishers' respective identity services. This affords both Platform and Publisher direct ownership of the respective user's accounts and enhanced privacy for the end user.

![Create Sub Sequence Diagram](paket_create_sub_seq_diagram.jpg)


## Plans

Plans define the subscription offerings available through the Paket catalog, including pricing, billing cycles, and bundled services.

### List Plans

> Endpoint

```
GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/plans
```

Retrieves a list of available Plans from the catalog.

> GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/plans

```curl
curl --location 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/plans' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
    "items": [
        {
            "billing_frequency": {
                "unit": "month",
                "value": 1
            },
            "free_trial_days": 0,
            "plan_type": "sub_bundle",
            "updated_at": "2025-08-15T14:59:34+00:00",
            "platform_id": "PL468440696748511232",
            "name": "Disney+, Hulu, HBO Max Bundle",
            "media": {
                "promo_4k_1x": "https://media-dev.paket.tv/media/plans/PL468440696748511232/427944e5ba9e/promo_4k@1x.png",
                "promo_2k_1x": "https://media-dev.paket.tv/media/plans/PL468440696748511232/427944e5ba9e/promo_2k@1x.png"
            },
            "plan_id": "427944e5ba9e",
            "status": "active",
            "localizations": {
                "en-us": {
                    "description": "The bundle with everything you need",
                    "display_name": "Disney+, Hulu, HBO Max Bundle"
                }
            },
            "prices": {
                "US": [
                    {
                        "order": 1,
                        "billing_cycles": null,
                        "price": {
                            "price_in_cents": 1699,
                            "tier_id": "1699",
                            "currency_code": "USD",
                            "price": 16.99
                        }
                    }
                ]
            }
            "created_at": "2025-07-20T04:17:16+00:00",
            "updated_at": "2025-08-20T02:11:13+00:00",
        }
    ],
    "total": 1,
    "next_key": null
}
```
\* required

**Headers**

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**Query Parameters**

**`limit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Number of plans to return (1-100, default: 25).

**`next_key`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Pagination key from previous response.

**`language`** <span style='margin: 0 5px;font-size:.9em'>string or array</span>  
Optional. Language code(s) for localized content (default: ["en-us"]).

**`region`** <span style='margin: 0 5px;font-size:.9em'>string or array</span>  
Optional. Region code(s) to filter plans by availability (e.g., "US", "CA").

**Returns**

Returns an array of Plan objects (without `plan_items` array) available in the catalog.

### Get Plan

> Endpoint

```
GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/plans/:plan_id
```

Retrieves detailed information about a specific plan.

> GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/plans/:plan_id

```curl
curl --location 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/plans/427944e5ba9e' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "plan_id": "427944e5ba9e",
  "name": "Disney+, Hulu, HBO Max Bundle",
  "plan_type": "sub_bundle",
  "status": "active",
  "platform_id": "PL468440696748511232",
  "free_trial_days": 0,
  "grace_period_days": 7,
  "billing_frequency": {
    "unit": "month",
    "value": 1
  },
  "media": {
    "promo_4k_1x": "https://media.paket.tv/media/plans/PL468440696748511232/427944e5ba9e/promo_4k@1x.png",
    "promo_2k_1x": "https://media.paket.tv/media/plans/PL468440696748511232/427944e5ba9e/promo_2k@1x.png"
  },
  "prices": {
    "US": [
      {
        "order": 1,
        "billing_cycles": 3,
        "price": {
          "price_in_cents": 1699,
          "tier_id": "1699",
          "currency_code": "USD",
          "price": 16.99
        }
      }
    ]
  },
  "localizations": {
    "en-us": {
        "description": "The bundle with everything you need",
        "display_name": "Disney+, Hulu, HBO Max Bundle"
    }
  },
  "plan_items": [
    {
        "status": "active",
        "price_wholesale": {
            "price": 4.56,
            "price_in_cents": 456,
            "currency_code": "USD"
        },
        "name": "Disney+ Basic",
        "app_id": "AP468442205989113856",
        "product_id": "PR469716925099413504",
        "localizations": {
            "en-us": {
                "description": "Disney+ Basic With Ads",
                "display_name": "Disney+ Basic"
            }
        },
        "prices": {
            "US": {
                "price_in_cents": 999,
                "tier_id": "999",
                "currency_code": "USD",
                "price": 9.99
            }
        },
        "app": {
            "id": "AP468442205989113856",
            "name": "Disney+",
            "media": {
                "icon_1x": "https://media.paket.tv/media/apps/AP468442205989113856/icon@1x.png",
                "icon_2x": "https://media.paket.tv/media/apps/AP468442205989113856/icon@2x.png",
                "icon_3x": "https://media.paket.tv/media/apps/AP468442205989113856/icon@3x.png",
                "logo_dark_1x": "https://media.paket.tv/media/apps/AP468442205989113856/logo_dark@1x.png",
                "logo_light_1x": "https://media.paket.tv/media/apps/AP468442205989113856/logo_light@1x.png",
                "tile_1x": "https://media.paket.tv/media/apps/AP468442205989113856/tile@1x.png",
                "tile_2x": "https://media.paket.tv/media/apps/AP468442205989113856/tile@2x.png"
            },
            "status": "live"
        }
    },
    ...
  ],
  "created_at": "2024-01-15T10:30:00.000Z",
  "updated_at": "2024-01-17T07:28:00.000Z"
}
```
\* required

**Headers**

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**Path Parameters**

**`plan_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The unique identifier of the plan to retrieve.


**Query Parameters**

**`region`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Region for pricing information (default: "US").

**Returns**

Returns a detailed Plan object with complete pricing, plan items, and phase information.

## Subscriptions

Subscriptions represent active relationships between users and plans, managing billing cycles, activation, and payment status. In a typical configuration, Paket operates exclusively as the orchestration layer and the Platform acts as the Merchant of Record, determining when and if an invoice is taxable, in which juristiction, at what amount, and whether the tax should be applied exclusive or inclusive of the plan price. 

### Create Subscription

> Endpoint

```
POST https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions
```

Creates a new subscription for a session with automatic invoice generation.

> POST https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions

```curl
curl --location 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions?region=US' \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Basic <credentials>'
  --header 'Idempotency-Key: unique-operation-123' \
  --data '{
    "session_id": "session_456",
    "plan_id": "427944e5ba9e",
    "tax_rate": 0.0875,
    "tax_type": "sales_tax",
    "tax_jurisdiction": "CA-Los Angeles",
    "tax_behavior": "exclusive",
    "tax_note": "",
    "device_info": {
      "device_type": "roku",
      "device_id": "FA1234567890"
    },
    "metadata": {
      "source": "homepage_banner",
      "campaign": "summer_promo"
    }
  }'
``` 

> Response

```http
HTTP/1.1 201 Created
Content-Type: application/json
{
  "subscription": {
    "subscription_id": "SUB479027832035610624",
    "session_id": "session_456",
    "plan_id": "427944e5ba9e",
    "plan_name": "Disney+, Hulu, HBO Max Bundle",
    "plan_type": "sub_bundle",
    "status": "pending",
    "activation_status": "pending",
    "payment_status": "unpaid",
    "next_billing_date": "2025-09-14T20:45:35.064Z",
    "current_period_start": "2025-08-14T20:45:35.065Z",
    "current_period_end": "2025-09-14T20:45:35.064Z",
    "trial_end": null,
    "grace_period_days": 7,
    "grace_period_end": "2025-09-21T20:45:35.064Z",
    "billing_frequency": {
      "unit": "month",
      "value": 1
    },
    "free_trial_days": 0,
    "tax_rate": 0.0875,
    "tax_type": "sales_tax",
    "tax_jurisdiction": "CA-Los Angeles",
    "tax_behavior": "exclusive",
    "tax_note": "",
    "cancel_at_period_end": false,
    "canceled_at": null,
    "payment_method_id": null,
    "activation_url": null,
    "activation_token": null,
    "platform_id": "PL468440696748511232",
    "metadata": {
      "source": "homepage_banner",
      "campaign": "summer_promo"
    },
    "device_info": {
      "device_type": "roku",
      "device_id": "FA1234567890"
    },
    "created_at": "2025-08-14T20:45:35.065Z",
    "updated_at": "2025-08-14T20:45:35.065Z"
  },
  "invoice": {
    "invoice_id": "INV479027832278880256",
    "invoice_number": "INV-2025-78880256",
    "subscription_id": "SUB479027832035610624",
    "invoice_date": "2025-08-14T20:45:35.122Z",
    "due_date": "2025-09-13T20:45:35.122Z",
    "status": "open",
    "payment_status": "unpaid",
    "subtotal": 1699,
    "tax_amount": 149,
    "total_amount": 1848,
    "amount_due": 1848,
    "amount_paid": 0,
    "currency": "USD",
    "tax_rate": 0.0875,
    "tax_type": "sales_tax",
    "tax_jurisdiction": "CA-Los Angeles",
    "tax_behavior": "exclusive",
    "tax_note": "",
    "period_start": "2025-08-14T20:45:35.065Z",
    "period_end": "2025-09-14T20:45:35.064Z",
    "plan_id": "427944e5ba9e",
    "plan_name": "Disney+, Hulu, HBO Max Bundle",
    "plan_type": "sub_bundle",
    "session_id": "session_456",
    "platform_id": "PL468440696748511232",
    "retry_count": 0,
    "max_retries": 3,
    "payment_method_id": null,
    "payment_intent_id": null,
    "created_at": "2025-08-14T20:45:35.122Z"
  }
}
```

**Headers**

\* required

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**`Idempotency-Key`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional but recommended. Unique key to prevent duplicate subscriptions.

**Query Parameters**

**`region`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Pricing region (default: "US").

**Request Body**

\* required

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The user session identifier.

**`plan_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The plan to subscribe to.

**`tax_behavior`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Specifies whether tax should be collected and how (default: "none"). Valid values:

- `inclusive` - Tax will be included in plan price (subtotal recalculated based on tax)
- `exclusive` - Tax will be added to plan price
- `none` - Plan is not taxable

**`tax_rate`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Optional. Tax rate to apply as decimal percentage (0-1, default: 0).

**`tax_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Type of tax (default: "none"). Valid values:

- `sales_tax`
- `vat`
- `gst`
- `pst`
- `hst`
- `none`

**`tax_jurisdiction`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Tax jurisdiction code (e.g., "CA-Los Angeles").

**`tax_note`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Additional notes pertaining to the subscription tax.

**`device_info`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Optional. Device information for tracking and analytics.

**`metadata`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Optional. Additional metadata for the subscription.

**Returns**

Returns an Invoice object for the created subscription.

<aside class="notice">
Creating a subscription automatically generates the first invoice for the billing period. The subscription becomes active once the invoice is paid.
</aside>

### Update Subscription

> Endpoint

```
PUT https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id
```

Updates an existing subscription's status, payment method, or other properties.

> PUT https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id

```curl
curl --location --request PUT 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/SUB479027832035610624' \
  --header 'Content-Type: application/json' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
  --header 'Idempotency-Key: unique-update-456' \
  --data '{
    "status": "active",
    "payment_method_id": "pm_123",
    "activation_url": "https://activate.disney.com/paket/abc123",
    "activation_token": "token_xyz789"
  }'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "subscription_id": "SUB479027832035610624",
  "status": "active",
  "payment_status": "paid",
  "plan_id": "427944e5ba9e",
  "plan_name": "Disney+, Hulu, HBO Max Bundle",
  "next_billing_date": "2025-09-14T20:45:35.064Z",
  "current_period_start": "2025-08-14T20:45:35.065Z",
  "current_period_end": "2025-09-14T20:45:35.064Z",
  "billing_frequency": {
    "unit": "month",
    "value": 1
  },
  "tax_rate": 0.0875,
  "tax_type": "sales_tax",
  "cancel_at_period_end": false,
  "payment_method_id": "pm_123",
  "activation_url": "https://activate.disney.com/paket/abc123",
  "activation_token": "token_xyz789",
  "updated_at": "2025-08-14T21:00:00.000Z"
}
```

**Path Parameters**

\* required

**`subscription_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The subscription ID to update.

**Headers**

\* required

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**`Content-Type`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Must be `application/json`.

**`Idempotency-Key`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional but recommended. Unique key to prevent duplicate updates.

**Request Body**

At least one field is required.

**`status`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Update subscription status. Valid values:
- `trialing`
- `active`
- `past_due`
- `canceled`
- `unpaid`
- `paused`

**`payment_method_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Payment method identifier.

**`cancel_at_period_end`** <span style='margin: 0 5px;font-size:.9em'>boolean</span>  
Optional. Schedule cancellation at end of current period.

**`plan_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Change to a different plan.

**`activation_url`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Publisher activation URL.

**`activation_token`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Token for activation process.

**`tax_rate`** <span style='margin: 0 5px;font-size:.9em'>number</span>  
Optional. Update tax rate (0-1).

**`tax_type`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Update tax type.

**`tax_jurisdiction`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Update tax jurisdiction.

**`tax_behavior`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Update tax behavior.

**`tax_note`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Update tax note.

**`metadata`** <span style='margin: 0 5px;font-size:.9em'>object</span>  
Optional. Update metadata.

**Returns**

Returns the updated Subscription object.

### Get Subscription

> Endpoint

```
GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id
```

Retrieves details of a specific subscription.

> GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id

```curl
curl --location 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/SUB479027832035610624' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "subscription_id": "SUB479027832035610624",
  "session_id": "session_456",
  "plan_id": "427944e5ba9e",
  "plan_name": "Disney+, Hulu, HBO Max Bundle",
  "plan_type": "sub_bundle",
  "status": "active",
  "activation_status": "pending",
  "payment_status": "paid",
  "next_billing_date": "2025-09-14T20:45:35.064Z",
  "current_period_start": "2025-08-14T20:45:35.065Z",
  "current_period_end": "2025-09-14T20:45:35.064Z",
  "trial_end": null,
  "grace_period_days": 7,
  "billing_frequency": {
    "unit": "month",
    "value": 1
  },
  "free_trial_days": 0,
  "tax_rate": 0.0875,
  "tax_type": "sales_tax",
  "tax_jurisdiction": "CA-Los Angeles",
  "cancel_at_period_end": false,
  "canceled_at": null,
  "payment_method_id": "pm_123",
  "platform_id": "783203561062490278320",
  "metadata": {},
  "created_at": "2025-08-14T20:45:35.065Z",
  "updated_at": "2025-08-14T21:00:00.000Z"
}
```

**Headers**

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**Path Parameters**

\* required

**`subscription_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The subscription ID to retrieve.

**Returns**

Returns a complete Subscription object.

### List Subscriptions

> Endpoint

```
GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions
```

Lists subscriptions for a specific session.

> GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions

```curl
curl --location 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions?session_id=session_456&limit=50' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "subscriptions": [
    {
      "subscription_id": "SUB479027832035610624",
      "plan_id": "427944e5ba9e",
      "plan_name": "Disney+, Hulu, HBO Max Bundle",
      "status": "active",
      "payment_status": "paid",
      "next_billing_date": "2025-09-14T20:45:35.064Z",
      "total_amount_due": 1848,
      "currency": "USD",
      "created_at": "2025-08-14T20:45:35.065Z"
    },
    {
      "subscription_id": "SUB479025528242835456",
      "plan_id": "netflix_standard",
      "plan_name": "Netflix Standard",
      "status": "trialing",
      "payment_status": "no_payment_required",
      "next_billing_date": "2025-08-21T10:30:00.000Z",
      "total_amount_due": 0,
      "currency": "USD",
      "created_at": "2025-08-07T10:30:00.000Z"
    }
  ],
  "lastEvaluatedKey": null
}
```

**Headers**

\* required

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**Query Parameters**

**`session_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The session ID to list subscriptions for.

**`limit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Number of results to return (1-100, default: 25).

**Returns**

Returns an array of summarized Subscription objects.

## Invoices

Invoices track billing statements and payment collection for subscription periods.

### Update Invoice

> Endpoint

```
PUT https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id/invoices/:invoice_id
```

Updates an invoice's payment status and related fields.

> PUT https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id/invoices/:invoice_id

```curl
curl --location --request PUT 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/SUB479027832035610624/invoices/INV479027832278880256' \
  --header 'Content-Type: application/json' \
  --header 'Authorization: Basic <credentials>'
  --header 'Idempotency-Key: unique-payment-789' \
  --data '{
    "payment_status": "paid",
    "payment_method_id": "pm_123",
    "payment_intent_id": "pi_456"
  }'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "invoice_id": "INV479027832278880256",
  "invoice_number": "INV-2025-78880256",
  "subscription_id": "SUB479027832035610624",
  "status": "paid",
  "payment_status": "paid",
  "subtotal": 1699,
  "tax_amount": 149,
  "total_amount": 1848,
  "amount_paid": 1848,
  "amount_due": 0,
  "currency": "USD",
  "payment_method_id": "pm_123",
  "payment_intent_id": "pi_456",
  "paid_at": "2025-08-14T21:15:00.000Z",
  "retry_count": 0,
  "updated_at": "2025-08-14T21:15:00.000Z"
}
```

\* required

**Headers**

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**`Idempotency-Key`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional but recommended. Unique key to prevent duplicate updates.

**Path Parameters**

**`subscription_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The subscription ID.

**`invoice_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The invoice ID to update.

**Request Body**

At least one field is required.

**`payment_status`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Update payment status. Valid values:
- `open`
- `paid`
- `failed`
- `processing`
- `canceled`

**`payment_method_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Payment method used.

**`payment_intent_id`** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Optional. Payment processor intent ID.

**Returns**

Returns the updated Invoice object.

<aside class="notice">
When payment_status is set to "paid", the invoice status automatically updates to "paid" and the subscription becomes active if it was pending payment.
</aside>

### Get Invoice

> Endpoint

```
GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id/invoices/:invoice_id
```

Retrieves details of a specific invoice.

> GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id/invoices/:invoice_id

```curl
curl --location 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/SUB479027832035610624/invoices/INV479027832278880256' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "invoice_id": "INV479027832278880256",
  "invoice_number": "INV-2025-78880256",
  "subscription_id": "SUB479027832035610624",
  "invoice_date": "2025-08-14T20:45:35.122Z",
  "due_date": "2025-09-13T20:45:35.122Z",
  "status": "open",
  "payment_status": "unpaid",
  "subtotal": 1699,
  "tax_amount": 149,
  "total_amount": 1848,
  "amount_due": 1848,
  "amount_paid": 0,
  "currency": "USD",
  "tax_rate": 0.0875,
  "tax_type": "sales_tax",
  "tax_jurisdiction": "CA-Los Angeles",
  "period_start": "2025-08-14T20:45:35.065Z",
  "period_end": "2025-09-14T20:45:35.064Z",
  "plan_id": "427944e5ba9e",
  "plan_name": "Disney+, Hulu, HBO Max Bundle",
  "retry_count": 0,
  "max_retries": 3,
  "next_retry_date": null,
  "session_id": "session_456",
  "platform_id": "783203561062490278320",
  "created_at": "2025-08-14T20:45:35.122Z"
}
```
\* required

**Headers**

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**Path Parameters**

**`subscription_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The subscription ID.

**`invoice_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The invoice ID to retrieve.

**Returns**

Returns a complete Invoice object.

### List Invoices

> Endpoint

```
GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id/invoices
```

Lists all invoices for a specific subscription.

> GET https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/:subscription_id/invoices

```curl
curl --location 'https://b3vevnva7a.execute-api.us-west-2.amazonaws.com/dev/v1/catalog/subscriptions/SUB479027832035610624/invoices?limit=50' \
  --header 'Accept: application/json' \
  --header 'Authorization: Basic <credentials>'
``` 

> Response

```http
HTTP/1.1 200 OK
Content-Type: application/json
{
  "invoices": [
    {
      "invoice_id": "INV479027832278880256",
      "invoice_number": "INV-2025-78880256",
      "invoice_date": "2025-08-14T20:45:35.122Z",
      "due_date": "2025-09-13T20:45:35.122Z",
      "status": "open",
      "payment_status": "unpaid",
      "total_amount": 1848,
      "currency": "USD",
      "period_start": "2025-08-14T20:45:35.065Z",
      "period_end": "2025-09-14T20:45:35.064Z"
    },
    {
      "invoice_id": "INV479025528485658880",
      "invoice_number": "INV-2025-85658880",
      "invoice_date": "2025-07-14T20:45:00.000Z",
      "due_date": "2025-08-13T20:45:00.000Z",
      "status": "paid",
      "payment_status": "paid",
      "total_amount": 1848,
      "currency": "USD",
      "period_start": "2025-07-14T20:45:00.000Z",
      "period_end": "2025-08-14T20:45:00.000Z"
    }
  ],
  "lastEvaluatedKey": null
}
```
\* required


**Headers**

**`Authorization`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
Your API credentials for authentication.

**Path Parameters**

**`subscription_id`*** <span style='margin: 0 5px;font-size:.9em'>string</span>  
The subscription ID to list invoices for.

**Query Parameters**

**`limit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
Optional. Number of results to return (1-100, default: 25).

**Returns**

Returns an array of summarized Invoice objects, sorted by most recent first.