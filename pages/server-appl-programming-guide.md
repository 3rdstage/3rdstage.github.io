---
layout: default
title: Server Application Programming Guide
category: programming guideline
toc: true
---

Server Application Programming Guide
---------

### Common Guideline

* Use English for Java API documentation (Javadoc comment) and comments as possible.
* Use annotations actively.
* Define collection type return should be non-null always. For no result, return empty collection.
* Use `final` modifier actively for method parameters and local variables to improve code readability and safety.

### Naming Convention

   | Category | Item | Name Pattern | Samples | Remarks |
   | -------- | ---- | ------------ | ------- | ------- |
   | Class    | Spring Controller Class | <tt>*Subejct*Controller</tt>  | UserController, ActivityController, RewardController |   |
   |          | Spring Service Class    | <tt>*Subject*Service</tt>    |                                                      |   |
   |          | MyBatis Mapper Class    | <tt>*Subject*Mapper</tt>     |                                                      |   |
   |          | MyBatis SQL Map         | <tt>*Subject*Mapper.xml</tt> |                                                      |   |
   | Controller Method | Finding/Listing Method | `findSubjectById` | findUserById                |   |
   |                   |                        | `findSubjectsByCategory` | findRewardsByUser    |   | 
   |                   |                        | `findSubjectsWithInterval` |                    |   |
   |                   | Creation Method        | `addSubject`               |                    |   |
   |                   | Update   Method        | `updateSubject`            |                    |   |
   |                   | Updating Single Proprety | `setPropertyOfSubject`   |                    |   |
   |                   | Deletion Method        | `removeSubject(s)`         |                    |   |
    

### Controller Programming Guide

#### Class Level

* Define one controller per resource as possible.
* Name with `Controller` postfix.
    * For example. `UserController`, `ActivityController`, `ActivityTypeController`, `CircleController`
    
* Define following annotations explicitly.

    | Annotation | Guideline |
    | ---------- | --------- |
    | `@RestController` |     |
    | `@RequestMapping` | Define base path for this controller and MIME type explicitly. |
    | `@Api`            | For Swagger API documentation. <br>Define class level base authorization requirement (usually administrative one) together. |
    | `@ParametersAreNonnullByDefault` | This annoation for just API documentation but not runtime automatic checking. |
    | `@ThreadSafe`     | Controllers are always supposed to be thread-safe without exceptional cases. | 

* Define the limits for page size or recent count as constants(`public final static` field).

* Sample
    ~~~java
@RestController
@RequestMapping(value = "/activities",
  produces = {MediaType.APPLICATION_JSON_UTF8_VALUE},
  consumes = {MediaType.APPLICATION_JSON_UTF8_VALUE})
@Api(value = "Activity",
  authorizations = {
      @Authorization(
        value = "defaultAuth",
        scopes = {
            @AuthorizationScope(scope = "admin", description = "")
        })
  })
@ParametersAreNonnullByDefault
@ThreadSafe
public class ActivityController{
 
  public static final int PAGE_SIZE_MAX = 100;
  public static final int PAGE_SIZE_MIN = 5;
  public static final String RECENT_COUNT_DEFAULT_STRING = "5";
  public static final int RECENT_COUNT_MAX = Constants.RECENT_COUNT_GLOBAL_MAX;
    ~~~
    
#### Method Level

* Name with following convention.

* Define following annotations explicitly.

    | Annotation | Guideline |
    | ---------- | --------- |
    | `@PostMapping`    | For creation type methods |
    | `@GetMapping`     | For listing or finding type methods |
    | `@PutMapping`     | For updating type methods |
    | `@DeleteMapping`  | For removing type methods |
    | `@ApiOperation`   | Add value element for Swagger API description explicitly. For non-administrative operation, add authorizations element explicitly. |
    | `@Nonnull` or `@Nullable` | The nullability of return should be defined explicitly. The throwable exceptions and return nullability considered together. |
    
* Use properly common model types such as `Count`, `Id`, `Value<T>` especially for return.

##### Find Method

* Define singular fetch operation using resource identifier in the first place explicitly.

* Definitely prefer paging
    * For normal resource (not code type resource), listing (collective fetch) operation should always be pagenated.
    * For normal resource, don't define "listing all" operation.
        * For code type resource (such as `ActivityType`, `RewardType`), define "listing all" operation 'cause mostly those resources are less than 100.

    * Don't include paging related word in method name 'cause paging is implicit in most cases.
    * Use `page-no` and `page-size` pair instead of `start-no` and `end-no` pair as paging parameters.
    * Define `page-size` parameter optional and provide default page size per controller or per resource type as a controller constant.
    * Define accompanying counting operation for each listing opeation
        * `findActivitiesWithInterval` / `countActivitiesWithInterval`
        * `findActivitiesByUserWithInterval` / `countActivitiesWithInterval`
        * `findActivitiesByUserAndType` / `countActivitiesByUserAndType`
    * Name path for counting operation to end with `/count`
        * `/activities/count`
        * `/activities/belongTo/{userId}/count`
        * `/activities/belongTo/{userId}/typed/{activityTypeCode}/count`
        
* Use `belongTo/{userId}` path element for per-user listing operation
    * Don't use preceding `/users/{userId}` path element (such as `/users/{userId}/activities`, `/users/{userId}/rewards`).

* Define sort of listing operation using sort query parameter.

* Sample
    ~~~java
  @RequestMapping(method = RequestMethod.GET, value = "/{activityId}")
  @ApiOperation(value = "Find the specified activity",
    authorizations = {
        @Authorization(
          value = "defaultAuth",
          scopes = {
              @AuthorizationScope(scope = "member", description = "")
          })
    })
  @Nullable
  public Activity findActivityById(@PathVariable("activityId") final String id){
    ...
  }
  @RequestMapping(method = RequestMethod.GET, value = "/belongTo/{userId}/count")
  @ApiOperation(value = "Count activities for a user within the specified interval",
    authorizations = {
        @Authorization(value = "defaultAuth", scopes = {
            @AuthorizationScope(scope = "member", description = "")
        })
    })
  @Nonnull
  public Count countActivitiesByUserWithInterval(
    @PathVariable("userId") @ApiParam("the user who owns the found activities") String userId,
    @RequestParam("start") @ApiParam("the start date of interval in 'yyyyMMddHHmmss' format") String start,
    @RequestParam("end") @ApiParam("the end date of interval in 'yyyyMMddHHmmss' format") String end){
    ...
  }
  @RequestMapping(method = RequestMethod.GET, value = "/belongTo/{userId}")
  @ApiOperation(value = "List activities for a user within the specified interval in page",
    authorizations = {
        @Authorization(value = "defaultAuth", scopes = {
            @AuthorizationScope(scope = "member", description = "")
        })
    })
  @Nonnull
  public List<Activity> findActivitiesByUserWithInterval(
    @PathVariable("userId") @Size(max = 100) @Pattern(regexp = "[a-z]*") String userId,
    @RequestParam("start") @ApiParam("the start date of interval in 'yyyyMMddHHmmss' format") String start,
    @RequestParam("end") @ApiParam("the end date of interval in 'yyyyMMddHHmmss' format") String end,
    @RequestParam("pageNo") @ApiParam("the page to fetch") @Min(1) int pageNo,
    @RequestParam(value = "pageSize", required = false, defaultValue = PAGE_SIZE_DEFAULT_STRING) @ApiParam(
      defaultValue = PAGE_SIZE_DEFAULT_STRING,
      value = "the size of page") @Min(PAGE_SIZE_MIN) @Max(PAGE_SIZE_MAX) int pageSize){
    Validator.validate(StringUtils.isNotEmpty(userId), "msg.user.unspecified", userId);
    Validator.validateInterval(start, end);
    Validator.validatePageSpec(pageNo, pageSize, PAGE_SIZE_MIN, PAGE_SIZE_MAX);
    ...
  }
  @RequestMapping(method = RequestMethod.GET, value = "/count")
  @ApiOperation(value = "List activities within the specified interval")
  @Nonnull
  public Count countActivitiesWithInterval(
    @RequestParam("start") @ApiParam("the start date of interval in 'yyyyMMddHHmmss' format") String start,
    @RequestParam("end") @ApiParam("the end date of interval in 'yyyyMMddHHmmss' format") String end){

    ...
  }
  @RequestMapping(method = RequestMethod.GET)
  @ApiOperation(value = "List activities within the specified interval")
  @Nonnull
  public List<Activity> findActivitiesWithInterval(
    @RequestParam("start") @ApiParam("the start date of interval in 'yyyyMMddHHmmss' format") String start,
    @RequestParam("end") @ApiParam("the end date of interval in 'yyyyMMddHHmmss' format") String end,
    @RequestParam("pageNo") @ApiParam("the page to fetch") @Min(1) int pageNo,
    @RequestParam(value = "pageSize", required = false, defaultValue = PAGE_SIZE_DEFAULT_STRING) @ApiParam(
      defaultValue = PAGE_SIZE_DEFAULT_STRING,
      value = "the size of page") @Min(PAGE_SIZE_MIN) @Max(PAGE_SIZE_MAX) int pageSize){
    ...
  }
    ~~~    
    