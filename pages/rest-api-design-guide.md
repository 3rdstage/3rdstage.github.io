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



