
## HTTP Methods, Headers, and Statuses  

* [HTTP Request, Response](#http-request-response)
* [HTTP Methods](#http-methods)
* [HTTP Status Codes](#http-status-codes)
* [Allowed Status Codes List](#allowed-status-codes-list)
* [Error Schema](#error-schema)
  
### HTTP Request, Response
  
We would assume that [JSON Schema](http://json-schema.org/) is used to describe request/response models.  
  
**Create user `POST` request example**  
```json  
{  
  "username": "somuser@usermail.com",  
  "password": "12313"  
}  
```  
  
**Success Response example**  
```json  
{  
  "success": true,  
  "data": {  
    "id": 145,  
    "username": "somuser@usermail.com"  
  }
}
```  
  
**Error Response example (see [Error Schema](#error-schema))**  
```json  
{  
  "success": false,  
  "message": "User already exists",  
  "details": [
    {  
        "field": "username",  
        "value": "somuser@usermail.com",  
        "issue": "User already exists"  
    }
  ]  
}  
```  
  
### HTTP Methods  
  
|HTTP Method|Description|  
|---|---|  
| `GET`| To _retrieve_ a resource. |  
| `POST`| To _create_ a resource, or to _execute_ a complex operation on a resource. |  
| `PUT`| To _update_ a resource. |  
| `DELETE`| To _delete_ a resource. |  
| `PATCH`| To perform a _partial update_ to a resource. |  
  
  
### HTTP Status Codes  
  
When responding to API requests, the following status code ranges MUST be used.  
  
|Range|Meaning|  
|---|---|  
|`2xx`|Successful execution. It is possible for a method execution to succeed in several ways. This status code specifies which way it succeeded.|  
|`4xx`|Usually these are problems with the request, the data in the request, invalid authentication or authorization, etc. In most cases the client can modify their request and resubmit.|  
| `5xx`| Server error: The server was not able to execute the method due to site outage or software defect. 5xx range status codes SHOULD NOT be utilized for validation or logical error handling. |  
  
### Allowed Status Codes List  
  
All REST APIs MUST use only the following status codes. Status codes in **`BOLD`** SHOULD be used by API developers. The rest are primarily intended for web-services framework developers reporting framework-level errors related to security, content negotiation, etc.  
  
* APIs MUST NOT return a status code that is not defined in this table.  
* APIs MAY return only some of status codes defined in this table.  
  
| Status Code | Description |  
|-------------|-------------|  
| **`200 OK`** | Generic successful execution. |  
| **`201 Created`** | Used as a response to `POST` method execution to indicate successful creation of a resource. If the resource was already created (by a previous execution of the same method, for example), then the server should return status code `200 OK`. |  
| **`202 Accepted`** | Used for asynchronous method execution to specify the server has accepted the request and will execute it at a later time. For more details, please refer [Asynchronous Operations](patterns.md#asynchronous-operations). |  
| **`204 No Content`** | The server has successfully executed the method, but there is no entity body to return.|  
| **`400 Bad Request`** | The request could not be understood by the server. Use this status code to specify:<br/> <ul><li>The data as part of the payload cannot be converted to the underlying data type.</li><li>The data is not in the expected data format.</li><li>Required field is not available.</li><li>Simple data validation type of error.</li></ul>|  
| `401 Unauthorized` | The request requires authentication and none was provided. Note the difference between this and `403 Forbidden`. |  
| **`403 Forbidden`** | The client is not authorized to access the resource, although it may have valid credentials. API could use this code in case business level authorization fails. For example, accound holder does not have enough funds. |  
| **`404 Not Found`** | The server has not found anything matching the request URI. This either means that the URI is incorrect or the resource is not available. For example, it may be that no data exists in the database at that key. |  
| `405 Method Not Allowed` | The server has not implemented the requested HTTP method. This is typically default behavior for API frameworks.  
| `406 Not Acceptable` | The server MUST return this status code when it cannot return the payload of the response using the media type requested by the client. For example, if the client sends an `Accept: application/xml` header, and the API can only generate `application/json`, the server MUST return `406`. |  
| `415 Unsupported Media Type` | The server MUST return this status code when the media type of the request's payload cannot be processed. For example, if the client sends a `Content-Type: application/xml` header, but the API can only accept `application/json`, the server MUST return `415`. |  
| **`422 Unprocessable Entity`** | The requested action cannot be performed and may require interaction with APIs or processes outside of the current request. This is distinct from a 500 response in that there are no systemic problems limiting the API from performing the request. |  
| `429 Too Many Requests` | The server must return this status code if the rate limit for the user, the application, or the token has exceeded a predefined value. Defined in Additional HTTP Status Codes [RFC 6585](https://tools.ietf.org/html/rfc6585). |  
| **`500 Internal Server Error`** | This is either a system or application error, and generally indicates that although the client appeared to provide a correct request, something unexpected has gone wrong on the server. A `500` response indicates a server-side software defect or site outage. `500` SHOULD NOT be utilized for client validation or logic error handling. |  
| `503 Service Unavailable` | The server is unable to handle the request for a service due to temporary maintenance. |  
  
### Error Schema  
  
An error response include the following fields:  
  
* `message`: A human-readable message, describing the error. This message MUST be a description of the problem NOT a suggestion about how to fix it.  
* `details`: An array that contains individual instance(s) of the error with specifics such as the following. This field is required for client side errors (`4xx`).  
   * `field`: Name of the path parameter, field name in create/update request or query parameter.  
   * `value`: Value of the field in error.  
   * `issue`: Reason for error.  
* `debug_id`: A unique error identifier generated on the server-side and logged for correlation purposes (not required).