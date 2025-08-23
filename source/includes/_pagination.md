# Pagination

Many of our top-level API resources have support for bulk fetches through "list" API methods. For example, you can list Session Participants, Session Lists, and Participants' Lists. These list API methods share a common structure and can accept the following two parameters: **`limit`** and **`next_key`**.

## Pagination Response Format

> Response Example

```json
{
    "total": 24,
    "next_key": 6,
    "items": [
        {...},
        {...},
        {...},
        {...},
        {...}
    ]
}
```

### Parameters

**`limit`** <span style='margin: 0 5px;font-size:.9em'>integer</span>  
The `limit` parameter is provided at the time of the request and limits the number of items returned in the API response. By default, this value is **50**.

**`next_key`** <span style='margin: 0 5px;font-size:.9em'>string/integer</span>  
The `next_key` parameter is returned in a response in which the total results exceed the default or specified `limit` and specifies the next item to be returned in a subsequent API request. Alternatively, a request can be made with any `next_key` value, however, there is no guarantee such an object will exist.