# Idempotency

The Paket API supports idempotency for safely retrying requests without accidentally performing the same operation twice. When creating or updating an object, use an idempotency key. Then, if a connection error occurs, you can safely repeat the request without risk of creating a second object or performing the update twice.

To perform an idempotent request, provide an additional `Idempotency-Key` element to the request header.

Paket's idempotency works by saving the resulting status code and body of the first request made for any given idempotency key, regardless of whether it succeedes or fails. Subsequent requests with the same key return the same result.

A requesting Client generates an idempotency key, which is a unique key that the server uses to recognize subsequent retries of the same request. How you create unique keys is up to you, but we suggest using V4 UUIDs, or another random string with enough entropy to avoid collisions. Idempotency keys are up to 255 characters long.

We remove keys from the system automatically after they’re at least 24 hours old and will generate a new request if a key is reused after the original is pruned. The idempotency layer compares incoming parameters to those of the original request and errors if they’re the same to prevent accidental misuse.

We save results only after the execution of an endpoint begins. If incoming parameters fail validation, or the request conflicts with another request that’s executing concurrently, we don’t save the idempotent result because no API endpoint initiates the execution. You can retry these requests.

All POST requests accept idempotency keys. Don’t send idempotency keys in GET and DELETE requests because it has no effect. These requests are idempotent by definition.