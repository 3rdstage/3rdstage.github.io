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
  | **Date** | Represents general exact full temporal point. Generally resolves from year upto second corresponding to '`yyyy-MM-dd HH:mm:ss`' format. Alias : Timestamp |   |
  | **Time** | Represents time only part. Generally resolves from hour upto second corresponding to '`HH:mm:ss`' format. |   |
  | **Interval** | Represents the intervening time between two temporal points. Includes explicit start point and/or end point. For open interval, one point can be excluded. Corresponds to '`yyyy-MM-dd HH:mm:ss ~ yyyy-MM-dd HH:mm:ss`' format. | [ISO 8601 / Time Intervals](https://en.wikipedia.org/wiki/ISO_8601#Time_intervals) |
  | **Duration** | Represents the amount of intervening time in a temporal interval. | [ISO 8601 / Durations](https://en.wikipedia.org/wiki/ISO_8601#Durations) |



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


   