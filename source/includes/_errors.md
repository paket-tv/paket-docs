# Errors

Paket uses conventional HTTP response codes to indicate the success or failure of an API request. 

## HTTP Status Codes

In general:
- Codes in the **2xx** range indicate success
- Codes in the **4xx** range indicate an error that failed given the information provided (e.g., a required parameter was omitted, etc.)
- Codes in the **5xx** range indicate an error with Paket's servers

### Common Error Codes

| Error Code | Meaning |
|------------|---------|
| **400** | Bad Request -- Your request is invalid. |
| **401** | Unauthorized -- Your API credentials are wrong. |
| **403** | Forbidden -- You do not have permission to access the requested resource. |
| **404** | Not Found -- The specified resource could not be found. |
| **500** | Internal Server Error -- We had a problem with our server. Try again later. |
| **503** | Service Unavailable -- We're temporarily offline for maintenance. Please try again later. |