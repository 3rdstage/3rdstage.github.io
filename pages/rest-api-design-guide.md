---
layout: default
title: REST API Design Guide
category: programming guideline
toc: true
---

REST API Design Guide
---------

### Common Guideline

* Use only JSON(`application/json`) for both for input and output body.
* Use HTTPS not HTTP.
* Prefer query parameters or template parameters and do NOT use matrix parameters or header parameters as possible.
* Avoid find all API
* Use only English for descriptions

### Common Concepts

#### Date, Time, Interval, Duration

* [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601)
* [Joda-Time Key Concepts](http://www.joda.org/joda-time/key.html)
* [SQL-99 Temporal Values](https://mariadb.com/kb/en/sql-99/08-temporal-values/)

  | Concept | Description | Remarks |
  | ------- | ----------- | ------- |
  | **Datetime** | Represents general exact full temporal point. Generally resolves from year upto second corresponding to '`yyyy-MM-dd HH:mm:ss`' format. Alias : Timestamp |   |
  | **Date** | Represents date only part. Generally resolves from year upto day corresponding to '`yyyy-MM-dd`' format
  | **Time** | Represents time only part. Generally resolves from hour upto second corresponding to '`HH:mm:ss`' format. |   |
  | **Interval** | Represents the intervening time between two temporal points. Includes explicit start point and/or end point. For open interval, one point can be excluded. Corresponds to '`yyyy-MM-dd HH:mm:ss ~ yyyy-MM-dd HH:mm:ss`' format. | [ISO 8601 / Time Intervals](https://en.wikipedia.org/wiki/ISO_8601#Time_intervals) |
  | **Duration** | Represents the amount of intervening time in a temporal interval. | [ISO 8601 / Durations](https://en.wikipedia.org/wiki/ISO_8601#Durations) |

#### State or Status

* 'State' and 'Status' is very similar and it is meaningless trying to distinguish them.
* Just select one.

### Domain Modeling

* **Clarity** over Flexibility

* Resources Relationship
    * Relationship type
        * **Mapping** over Hierarchy
    * Fundamental resources vs Complex resources : 
        * `User`, `Product`
        * `Sales`, `Order`
    * Relationship representation
        * By path : `/users/{userId}/orders`
        * By parameter : `/orders?userId={userId}`
    * 1:0...1 Separation

* Authentication and Authorization
    * **Role** over Privilege

* Avoid too general resource names
    * `elements`, `entries`, `items`
    * `instances`, `objects`, `resources`
    * `types`, `values`

#### Naming Convention

  | Element | Convention | Remarks |
  | ------- | ---------- | ------- |
  | Resource Collection Name | lowerCamelCase |
  
#### Common Abbreviation

  | Abbreviated Name | Full Name           | Remarks |
  | ---------------- | ------------------- | ------- |
  | id               | identifier          |         |
  | spec             | specification       |         |
  | stats            | statistics          |         |
  | req              | request             |         |
  | resp             | response            |         |  

### Header

* [List of HTTP header fields](https://en.wikipedia.org/wiki/List_of_HTTP_header_fields)

#### Custom Request Header

   | Header Field | Description | Value Space | Default | Sample | Remarks |
   | ------------ | ----------- | ----------- | ------- | ------ | ------- |
   | `X-User-Locale` |          |             |         |        |         |
   | `X-User-Timezone` |        |             |         |        |         |
   | `X-Client-Can-Build-Message` |           |         |        |         |
   | `X-Auth-Token` | a token value for API access which was gained from authentication process |   |   | 5aded866-9248-4058-879d-9266bdc4cd02 |   |

#### Custom Response Header

   | Header Field | Description | Value Space | Default | Sample | Remarks |
   | ------------ | ----------- | ----------- | ------- | ------ | ------- |
   | `X-Rate-Limit-Limit` | the rate limit ceiling for that given endpoint | positive integer |   |   |   |
   | `X-Rate-Limit-Remaining` | the number of requests left for the current time window | non-negative integer |   |   |   |
   | `X-Rate-Limit-Reset` | the remaining window before the rate limit resets, in UTC epoch seconds | non-negative integer  |   |   |
   | `X-Page-Ended` |           | boolean     | true    |        |         |
   
* [Twitter API Rate Limits](https://developer.twitter.com/en/docs/basics/rate-limiting)
* [GitHub API Rate Limiting](https://developer.github.com/v3/#rate-limiting)

### Response Code

#### Successful Response

  | Code | Status | Description | Example |
  | ---- | ------ | ----------- | ------- |
  | **200** | **OK** | The request has succeeded. |   |
  | **201** | **Created** | The request has been fulfilled and resulted in a new resource being created. |   |
  | **202** | **Accepted** | The request has been accepted for processing, but the processing has not been completed. |   |
  | **204** | **No Content** | The server has fulfilled the request but does not need to return an entity-body, and might want to return updated
metainformation. |
  
* 200/OK
    * The information returned with the response is dependent on the method used in the request, for example:
    
      | Method | Return |
      | ------ | ------ |
      | GET    | an entity corresponding to the requested resource is sent in the response |
      | HEAD   | the entity-header fields corresponding to the requested resource are sent in the response without any message-body |
      | POST   | an entity describing or containing the result of the action |
      | TRACE  | an entity containing the request message as received by the end server. |

* 201/Created
    * The newly created resource can be referenced by the URI(s) returned in the entity of the response, with the most specific URI for the resource given by a Location header field. The response SHOULD include an entity containing a list of resource characteristics and location(s) from which the user or user agent can choose the one most appropriate. The entity format is specified by the media type given in the Content-Type header field. The origin server MUST create the resource before returning the 201 status code. If the action cannot be carried out immediately, the server SHOULD respond with 202 (Accepted)
    * A 201 response MAY contain an ETag response header field indicating the current value of the entity tag for the requested variant just created response instead.
    
* 202/Accepted
    * The request might or might not eventually be acted upon, as it might be disallowed when processing actually takes place. There is no facility for re-sending a status code from an asynchronous operation such as this.
    * The 202 response is intentionally non-committal. Its purpose is to allow a server to accept a request for some other process (perhaps a batch-oriented process that is only run once per day) without requiring that the user agent's connection to the server persist until the process is completed. The entity returned with this response SHOULD include an indication of the request's current status and either a pointer to a status monitor or some estimate of when the user can expect the request to be fulfilled.


* 204/No Content
    * The response MAY include new or updated metainformation in the form of entity-headers, which if present SHOULD be associated with the requested variant.
    * If the client is a user agent, it SHOULD NOT change its document view from that which caused the request to be sent.
    * This response is primarily intended to allow input for actions to take place without causing a change to the user agent's active document view, although any new or updated metainformation SHOULD be applied to the document currently in the user agent's active view.
    * The 204 response MUST NOT include a message-body, and thus is always terminated by the first empty line after the header fields

#### Error Response

  | Cause | Code | Status | Processed By | Remarks |
  | ----- | ---- | ------ | ------------ | ------- |
  | When the client accesses undefined API(URL) | **404** | **Not Found** | Framework |   |
  | When the request is not 'application/json' type | **415** | **Unsupported Media Type** | Framework | What if the API doesn't need request body ? |
  | When the client is not properly authenticated | **401** | **Unauthorized** | Framework |   | 
  | When the client accesses the API that is not permitted to him/her | **403** | **Forbidden** | Framework |   |
  | When the client send JSON data that can't be parsed | **400** | **Bad Request** | Framework |   |
  | When the request doesn't contain required parameters | **400** | **Bad Request** | Framework |   |
  | When the request contains required parameters but has an invalid value among them | **400** | **Bad Request** | Controller |   |
  | When the resource (`User`, `Product`, `Order`, `Point`, ...) to find, update or remove doesn't exist | **404** | **Not Found** | Controller |   |
  | When the resource to add already exists |   |   |   |   |
  | When the server causes errors processing the request | **500** | **Internal Server Error** | Controller | network error, data access error |
  
* [List of HTTP status codes](https://en.wikipedia.org/wiki/List_of_HTTP_status_codes)
* [Twitter API Error Codes & Responses](https://developer.twitter.com/en/docs/basics/response-codes)
* [GitHub API Client Errors](https://developer.github.com/v3/#client-errors)

##### Consideration

* What is the optimized flow of processing ?
* What if the client requested with media types other than 'application/json' ?
* What if the normal user try to access other user's data ?
* Is it possible to provide intenationalized error message for errors automatically processed by framework

### Pattern, Best Practice

* (List Pagination)[https://cloud.google.com/apis/design/design_patterns#list_pagination]

#### Common Path Word And Query Parameter

  | Word                   | Description | Sample |
  | ---------------------- | ----------- | ------ |
  | `/count`               |             |        |
  | `/refresh`             |             |        |
  | `/belong-to/{userId}`  |             |        |
  | `/recent`              |             |        |
  | `/last`                |             |        |
  | `?pageSize={size)&pageNo={no}` |     |        |
  | `?sort={sortBy}`       |             |        |

#### Pagination

* Listing API should always be paginated.
* Parameters : 

    | Parameter | Description | Datatype | Value Space | Remarks |
    | --------- | ----------- | -------- | ----------- | ------- |
    | `pageSize`  | page size     | integer   | non-negative | `0` : the server or application default size |
    | `pageNo`    | page position | integer   | -1 or positive | `1` : first page, `-1` : last page         |

#### Base API

  | Title                                   | Method | URL                  | Req. Body | Resp. Body | Remakrs |
  | --------------------------------------- | ------ | -------------------- | --------- | ---------- | ------- |
  | Get current version of messages         | `GET`  | `/messages/version`  |           |            |         |
  | List all defined messages or message templates | `GET` | `/messages`    |           |            |         |
  | List all available locales              | `GET`  | `/locales`           |           |            |         |
  | List all available timezones            | `GET`  | `/timezones`         |           |            |         |
  | List developers of this application both current and past | `GET` | `/crews` |      |            |         |
  | List current developers of this application | `GET` | `/crews/current`  |           |            |         |
  | List past developers who is not working for this appl any more but .. | `GET` | `/crews/past` |  |    |    |
  | List release notes                      | `GET`  | `/releasenotes`      |           |            |         |

### Standard

#### HTTP Method Summary

  | Method    | RFC      | Request Has Body | Response Has Body | Safe | Idempotent | Cacheable |
  | --------- | -------- | :--------------: | :---------------: | :--: | :--------: | :-------: |
  | `GET`  | [RFC 7231](https://tools.ietf.org/html/rfc7231) |  No  |  Yes  |  Yes  |  Yes  |  Yes  |
  | `HEAD`    | RFC 7231 | No               | No                | Yes  | Yes        | Yes       |
  | `POST`    | RFC 7231 | Yes              | Yes               | No   | No         | Yes       |
  | `PUT`     | RFC 7231 | Yes              | Yes               | No   | Yes        | No        |
  | `DELETE`  | RFC 7231 | No               | Yes               | No   | Yes        | No        |
  | `CONNECT` | RFC 7231 | Yes              | Yes               | No   | No         | No        |
  | `OPTIONS` | RFC 7231 | Optional         | Yes               | Yes  | Yes        | No        |
  | `TRACE`   | RFC 7231 | No               | Yes               | Yes  | Yes        | No        |
  | `PATCH`   | [RFC 5789](https://tools.ietf.org/html/rfc5789) |  Yes  |  Yes  |  No  |  No  |  Yes  |

### Examples

#### User Service Example

##### REST API

  | Title | Method | URL | Body | Return | Remarks |
  | ----- | ------ | --- | ---- | ------ | ------- |
  | Add a user                | POST   | `roll/v1/users`                    | User(id=null)    |          |        |
  | List all users            | GET    | `roll/v1/users`                    |                  | User[]   |        |
  | List all valid users      | GET    | `roll/v1/users/valid`              |                  | User[]   |        |
  | List all invalid users    | GET    | `roll/v1/users/invalid`            |                  | User[]   |        |
  | List users of a specific organization | GET | `roll/v1/users?orgId={orgId}` |              | User[]   |        |
  | List valid users of a specified organization | GET | `roll/v1/valid?orgId={orgId}` |       | User[]   |        |
  | Find a user               | GET    | `roll/v1/users/{userId}`           |                  | User     |        |
  | Update a user             | PUT    | `roll/v1/users/{userId}`           | User             |          |        |
  | Make a user valid         | PUT    | `roll/v1/users/{userId}/valid`     |                  |          |        |
  | Make a user invalid       | PUT    | `roll/v1/users/{userId}/valid`     |                  |          |        |
  | Update a user's password  |        |                                    |                  |          |        |
  | Initialize a user's password |     |                                    |                  |          |        |
  | Get the hash of a user's password | GET | `roll/v1/users/{userId}/password/sha256`   |     | { "hash" : string }  |        |
  | Add a user email address  | POST   | `roll/v1/users/{userId}/emails`    |                  |          |        |
  | List all email addresses of a specified user | GET | `roll/v1/users/{userId}/emails` |     | { "email": string, "verified": boolean, "verifiedAt": date }[] |   |
  | Set a user email address verified | PUT | `roll/v1/users/{userId}/emails/{email}/verified` |    |          |        |
  | Remove a user email address | DELETE | `roll/v1/users/{userId}/emails/{email}`             |    |          |        |
  | Add a user phone number     | POST | `roll/v1/users/{userId}/phones`                       |          |        |
  | List all phone numbers of a specified user | GET | `roll/v1/users/{userId}/phones`   |     | { "phone": string, "verified": boolean, "verifiedAt": date }[] |   |
  | Set a user phone number verified | PUT | `roll/v1/users/{userId}/phones/{phoneNumber}/verified` |   |   |      |
  | Remove a user phone number | DELETE | `roll/v1/users/{userId}/phones/{phoneNumber}`  |     |          |        |
  | Add a user preference | POST | `roll/v1/users/{userId}/preference`      | { "key": string, "value": string }[] |    |
  | List all common preference keys | GET | `roll/v1/common/PreferenceKeys` |   | { "key": string, "description": string }[] |   |
  | Add a organization        | POSt   | `roll/v1/organizations`            | Org               |         |        |
  | List all organizations    | GET    | `roll/v1/organizations`            |                   | Org[]   |        |
  | Find an organization      | GET    | `roll/v1/organizations/{orgId}`    |                   | Org     |        |
  
##### Swagger API 

~~~~json
{
   "swagger": "2.0",
   "info": {
      "title": "API for authentication and authorization",
      "description": "The is a API service to manage users, organizations and their authentications and authorizations",
      "contact": {
         "name": "Sangmoon Oh",
         "email": "halfface@chollian.net"
      },
      "version": "0.7"
   },
   "basePath": "/roll/v1",
   "schemas": [
      "https"
   ],
   "consumes": [
      "application/json"
   ],
   "produces": [
      "application/json"
   ],
   "paths": {
      "/users": {
         "post": {
            "description": "Adds a new user",
            "parameters": {
               "name": "user",
               "in": "body",
               "description": "the user to add",
               "schema": {
                  "$ref": "#/definitions/user"
               }
            },
            "responses": {
               "204": {
                  "description": "a user successfully added"
               }
            }
         },
         "get": {
            "description": "Lists all users including invalid users",
            "responses": {
               "200": {
                  "description": "list of all users",
                  "schema": {
                     "$schema": "http://json-schema.org/draft-04/schema#",
                     "type": "array",
                     "items": {
                        "$ref": "#/definitions/user"
                     }
                  }
               }
            }
         }
      },
      "/users/valid": {
         "get": {
            "description": "Lists all valid users",
            "responses": {
               "200": {
                  "description": "list of all valid users",
                  "schema": {
                     "type": "array",
                     "items": {
                        "$ref": "#/definitions/user"
                     }
                  }
               }
            }
         }
      },
      "/users/invalid": {
         "get": {
            "description": "Lists all invalid users",
            "responses": {
               "200": {
                  "description": "list of all invalid users",
                  "schema": {
                     "type": "array",
                     "items": {
                        "$ref": "#/definitions/user"
                     }
                  }
               }
            }
         }
      }
   },
   "defintions": {
      "user": {
         "type": "object",
         "required": [
            "id",
            "name"
         ],
         "properties": {
            "id": {
               "description": "unique identifier of the user",
               "type": "string"
            },
            "name": {
               "description": "name of the user",
               "type": "string",
               "maxLength": 256
            },
            "isValid": {
               "description": "whether this user is valid and approved to access the system or not",
               "type": "boolean",
               "default": false
            },
            "passwordExpired": {
               "description": "whether or not the password of this user is expired and so the password rest is necessary",
               "type": "boolean",
               "default": false
            },
            "registeredAt": {
               "description": "the date-time when the user is registered - the value is in UTC00:00 or ",
               "type": "string",
               "format": "date-time",
               "pattern": "[0-9]{4}-[0-1][0-9]-[0-3][0-9]T[0-2][0-9]:[0-5][0-9]:[0-5][0-9]Z"
            },
            "expireAt": {
               "description": "the date-time when the user will be expired",
               "type": "string",
               "format": "date-time"
            },
            "email": {
               "description": "most favorite email address of the user",
               "type": "string",
               "format": "email"
            },
            "extraEmails": {
               "description": "extra email addresses of the user",
               "type": "array",
               "items": {
                  "type": "string",
                  "format": "email"
               }
            },
            "emailValidations": {
               "description": "email validation history of the user"
            },
            "phoneNumber": {
               "type": "string"
            },
            "extraPhoneNumbers": {
               "type": "array",
               "items": {
                  "type": "string"
               }
            },
            "phoneNumberValidations": {
               
            }
         }
      }
   }
}
~~~~


   