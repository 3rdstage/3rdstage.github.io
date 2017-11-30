---
layout: default
title: Simple Patterns and Typical Samples
description: ...
group: software developer
toc: true
---

Java
----

### JVM startup command-line

-   References
    -   [Java HotSpot VM Options](http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html)
    -   [JVM 8 Command-line Options](http://docs.oracle.com/javase/8/docs/technotes/tools/windows/java.html#CBBIJCHG)
    -   [Troubleshooting Guide for Java SE 6 with HotSpot VM](http://www.oracle.com/technetwork/java/javase/index-137495.html) (Nov. 2008)

``` dos
:: Starts stand-alone MyApp server
::
:: @author Sangmoon Oh
:: @since 2015-01-01

@ echo off
setLocal enableDelayedExpansion

if not exist "%JAVA_HOME%\bin\java.exe" (
  echo.
  echo Fail to run
  echo JDK 1.6 ^(32-bit^) or higher and 'JAVA_HOME' environment variable for it are required.
  goto :END
)
echo JAVA_HOME = %JAVA_HOME%

if exist "%JAVA_HOME%\release" (
  for /F "tokens=2 delims==" %%x in ('findstr "JAVA_VERSION" "%JAVA_HOME%\release"') do set JAVA_VERSION=%%~x
  for /F "tokens=2 delims==" %%x in ('findstr "OS_ARCH" "%JAVA_HOME%\release"') do set OS_ARCH=%%~x

  echo JAVA_VERSION = !JAVA_VERSION!
  echo OS_ARCH = !OS_ARCH!

  if !OS_ARCH! equ amd64 (
    echo.
    echo The provided JDK is 64-bit. This application requires 32-bit JDK.
    goto :END
  )
)

if not exist "%MYAPP_HOME%" (
  echo.
  echo 'MYAPP_HOME' environment variable is not set or has inappropriate value.
  goto :END
)
if not exist "%OPENCV_DIR%" (
  echo.
  echo 'OPENCV_DIR' environment variable is not set or has inappropriate value.
  goto :END
)
echo MYAPP_HOME = %MYAPP_HOME%
echo OPENCV_DIR = %OPENCV_DIR%

set CLASSPATH=%MYAPP_HOME%\conf;%MYAPP_HOME%\lib\*

:: For more on JVM options, refer http://www.oracle.com/technetwork/java/javase/tech/vmoptions-jsp-140102.html
set JVM_ARGS=-server -Xms256m -Xmx256m -XX:MaxPermSize=256M
set JVM_ARGS=%JVM_ARGS% -XX:NewRatio=2 -XX:SurvivorRatio=8
if %JAVA_VERSION:~0,3% equ 1.7 (
  set JVM_ARGS=%JVM_ARGS% -XX:+UseG1GC -XX:MaxGCPauseMillis=200 -XX:G1ReservePercent=10
) else (
  set JVM_ARGS=%JVM_ARGS%  -XX:+UseParallelGC -XX:+UseParallelOldGC -XX:+UseNUMA
)
set JVM_ARGS=%JVM_ARGS% -verbose:gc -Xloggc:"%MYAPP_HOME%\log\jvm\gc_%%t.log" -XX:+PrintGCDateStamps -XX:+PrintGCDetails
set JVM_ARGS=%JVM_ARGS% -XX:NativeMemoryTracking=detail
set JVM_ARGS=%JVM_ARGS% -XX:+UnlockDiagnosticVMOptions -XX:+PrintNMTStatistics
set JVM_ARGS=%JVM_ARGS% -XX:+DisableExplicitGC
set JVM_ARGS=%JVM_ARGS% -XX:+HeapDumpOnOutOfMemoryError -XX:HeapDumpPath="%MYAPP_HOME%\jvm"
set JVM_ARGS=%JVM_ARGS% -XX:ErrorFile="%MYAPP_HOME%\jvm\hs_err_pid%%p.log"
set JVM_ARGS=%JVM_ARGS% -Djava.library.path="%OPENCV_DIR%\..\..\java\x86"
set JVM_ARGS=%JVM_ARGS% -Dfile.encoding=UTF-8
set JVM_ARGS=%JVM_ARGS% -Duser.timezone=Asia/Seoul
rem set JVM_ARGS=%JVM_ARGS% -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=8000
set JVM_ARGS=%JVM_ARGS% -Dcom.sun.management.jmxremote.port=3333
set JVM_ARGS=%JVM_ARGS% -Dcom.sun.management.jmxremote.ssl=false
set JVM_ARGS=%JVM_ARGS% -Dcom.sun.management.jmxremote.authenticate=false
set JVM_ARGS=%JVM_ARGS% -Dlog.level.root=TRACE
set JVM_ARGS=%JVM_ARGS% -Dlog.level.spring=DEBUG
set JVM_ARGS=%JVM_ARGS% -Dlog.level.mybatis=DEBUG
set JVM_ARGS=%JVM_ARGS% -Dlog.timezone=Asia/Seoul

echo ************************************************
echo '"%JAVA_HOME%\bin\java" %JVM_ARGS% -cp "%CLASSPATH%" myapp.StandaloneServer'
echo ************************************************

"%JAVA_HOME%\bin\java" %JVM_ARGS% -cp "%CLASSPATH%" myapp.StandaloneServer

:END
endLocal
```

### Waiting **`Enter`** key press on the console

Using **`java.io.InputStream.read()`**, **`java.util.Scanner.hasNextLine()`**, or **`java.util.Scanner.nexLine()`** may make the code simple, but they are much bad on behalf of performance in concurrent environment or debugging situation. To make other thread or debugging session unaffected as possible, it is better to use **`java.io.InputStream.available()`** with loop.

```java
   ...
   System.out.println("Press [Enter] key to start.");
   int cnt;
   while((cnt = System.in.available()) < 1){
      Thread.sleep(500);
   }
   while(cnt-- > 0) System.in.read();
   ...

   System.out.println("Press [Enter] key to proceed.");
   while((cnt = System.in.available()) < 1){
      Thread.sleep(500);
   }
   while(cnt-- > 0) System.in.read();
   ...

   System.out.println("Press [Enter] key to end.");
   while((cnt = System.in.available()) < 1){
      Thread.sleep(500);
   }
   while(cnt-- > 0) System.in.read();
```

You should call the `available()` in the loop, because it is non blocking. The keys pressed before `Enter` should be consumed before the next wait.

-   Readings
    -   \[<http://docs.oracle.com/javase/6/docs/api/java/io/InputStream.html#available>() API of `java.io.InputStream.available()`\]

### **`wait`**, **`notify`** and **`notifyAll`**

The thread should own the monitor of the instance to call `wait`, `notify` or `notifyAll` before the call. So, the call of these basic method should be <strong>always located inside `synchronized` block</strong>.

A thread can wake up by unexpected or unpredictable reason, so the call of `wait` and the logic to process after wake-up should be guarded by loop.

So, the typical code block for calling `wait` is

``` java
   ...
   final Object obj = new Object;
   ...

   synchronized (obj) {
         while (<condition does not hold>)
             obj.wait(timeout);
         ... // Perform action appropriate to condition
   }

   ...
```

And for `notifyAll`

``` java
   ...
   final Object obj = new Object;
   ...

   synchronized (obj) {
      //Perform some action to release the hold condition
      obj.notifyAll();
      ...
   }

   ...
```

### Using <strong>`Lock`</strong> class of <strong>`java.util.concurrent.locks`</strong> package

``` java

 class X {
   private final ReentrantLock lock = new ReentrantLock();
   // ...

   public void m() {
     lock.lock();  // block until condition holds
     try {
       // ... method body
     } finally {
       lock.unlock()
     }
   }
 }
```

### Controlling start-up and detecting end of thread executions using `CountDownLatch`

``` java
class Control{

   private int numOfTasks;
   private CountDownLatch startCntDwn = new CountDownLatch(1);
   private CountDownLatch endCntDwn = new CountDownLatch(numOfTasks);

   //...

   public void execute() throws Exception{
      ExecutorService executor = Executors.newFixedThreadPool(numOfTasks);

      //conditions for the tasks
      List<Condition> conditions = new ArrayList<Condition>(numOfTasks);
      //tasks
      List<Task> tasks = new ArrayList<Task>(numOfTasks);
      //futures to get results
      List<Future<Result>> futures = new ArrayList<Future<Result>>(numOfTasks);
      //results of tasks
      List<Result> results = new ArrayList<Result>(numOfTasks);

      Condition cndt = null;
      for(int i = 0; i < numOfTasks; i++){
         cndt = new Condition();
         //setup Condition object for each task
         tasks.add(new Task(cndt, startCntDwn, endCntDwn));
      }

      for(Task task: tasks){
         futures.add(executor.submit(task));
      }

      startCntDwn.countDown();
      endCntDwn.await();

      for(Future<Result> future : futures){
         results.add(future.get());
      }

      //manipulating results

      executor.shutdown();
   }

   public long getNumberOfTasks(){ return this.numOfTasks; }

   public long getNumberOfRunningTasks(){
      if(this.startCntDwn.getCount() == 1) return 0;
      else return this.endCntDwn.getCount();
   }
}

class Task implements Callable<Result>{

   private CountDownLatch startCntDwn;
   private CountDownLatch endCntDwn;

   //other fields to do task.

   public Task(Condition condition, CountDownLatch start, CountDownLatch end){
      this.startCntDwn = start;
      this.endCntDwn = end;
   }

   public Result call() throws Exception{
      Result result = null;
      this.startCntDwn.await();

      //do its own task and create result;

      this.endCntDwn.countDown();
      return result;
   }
}

class Condition{

}

class Result{

}
```

### Lazy initialization of field

#### Lazy initialization of instance field using double-checked locking

Prior to JDK 1.5, double-checked locking is not safe with Java because the reordering of object publication and initialization can not be controlled. But as of JDK 1.5, volatile is specified not to be reordered, so it become possible to use safely double-checked locking with volatile field.

A typical code would be like this :

``` java
@ThreadSafe
public class Worker{

   private volatile Resource resource = null; //creating resource is expensive
   private final String lock = String.valueOf(-1);  //avoid String intern.

   @GuardedBy("lock")
   public Resource getResource(){
      Resource r = resource;
      if(r == null){ //first check
         synchronized(lock){
            r = resource;
            if(r == null){ //second check
               resource = r = new Resource();
            }
         }
      }
      return r;
   }

   ...
}
```

In above sample code, note that the `lock` object is define to be `final`, because reference on non final field can change any time. The above code can be enhanced using `java.util.concurrent.locks.Lock`

``` java
@ThreadSafe
public class Worker{

   private volatile Resource resource = null; //creating resource is expensive
   private final Lock lock = new ReentrantLock();

   @GuardedBy("lock")
   public Resource getResource(){
      Resource r = resource;
      if(r == null){ //first check
         lock.lock()
         try{
            r = resource;
            if(r == null){ //second check
               resource = r = new Resource();
            }
         }catch(Exception ex){
            //write log and re-throw the exception
         }finally{
        lock.unlock();
     }
      }
      return r;
   }

   ...
}
```

The following is a simple Spring container locator class.

``` java
/**
 * Locates Spring containers.
 * <p>
 * This class can be used to access Spring container from where DI is not possible.
 * <p>
 * Note that this class is thread-safe and caches the Spring containers
 *
 * @version 1.0, 2015-08-13, Sangmoon Oh, Initial version
 * @author Sangmoon Oh
 * @since 2015-08-13
 */
@Singleton
@ThreadSafe
public class SpringContainerLocator {

  private final Logger logger = LoggerFactory.getLogger(this.getClass());

  private static SpringContainerLocator instance;

  private volatile Map<String, ConfigurableApplicationContext> cache = new HashMap<String, ConfigurableApplicationContext>();

  private final Lock cacheLock = new ReentrantLock();

  /**
   * Test purpose filed to confirm concurrency in hight congestion.
   */
  private final AtomicInteger count = new AtomicInteger(0);

  /**
   * Gets the count of Spring container loadings.
   * This method is test only purpose.
   */
  protected int getCount(){ return this.count.intValue(); }

  static{
    //No need on lazy initialization.
    if(instance == null){ instance = new SpringContainerLocator(); }
  }

  private SpringContainerLocator(){}

  public static SpringContainerLocator getSingleton(){ return instance; }

  /**
   * Gets the Spring container configured by the specified configuration file.
   * <p>
   * Currently, the location of Spring configuration file should be
   * <emp>classpath relative</emp>.
   * <p>
   * The loaded Spring container will be registered shutdown hook, so you don't need to do in your code.
   *
   * @param configLoc The location of configuration file on classpath. such as 'foo/bar/spring.xml'
   * @return The Spring container configured by the specified configLoc.
   */
  @GuardedBy("cacheLock")
  public ApplicationContext getSpringContainer(@NotBlank String configLoc){
    Validate.isTrue(StringUtils.isNotBlank(configLoc), "The location of configuration should be specified.");
    ConfigurableApplicationContext container = cache.get(configLoc);

    if(container == null){ //first check
      cacheLock.lock();
      try{
        container = cache.get(configLoc);
        if(container == null){ //send check
          container = new ClassPathXmlApplicationContext(configLoc);
          container.registerShutdownHook();
          cache.put(configLoc, container);
          int cnt = this.count.incrementAndGet();
          logger.info("The {}-th loading of Spring container: {}", cnt, configLoc);
        }
      }catch(Exception ex){
        logger.error("Fail to load spring container whose configuration is" + configLoc + " on classpath.", ex);
        throw new RuntimeException("Fail to load spring container whose configuration is" + configLoc + " on classpath.", ex);
      }finally{
        cacheLock.unlock();
      }
    }

    return container;
  }
}
```

#### Lazy initialization of static field using initialization on demand holder class

On behalf of the characteristics of class loading and static initialization, static field can be safely lazy initialized using holder class without locking.

JVM load classes in lazy way and JVM run static initilizers at class initilization time which is after class loading but before the class is used by any thread. So, the static holder class having static field like the below would be lazy initialized in safe way.

``` java
public class Worker{

   private static class ResourceHolder{
      public static Resource resource = new Resource();
   }

   public static Resource getResource(){
      return ResourceHolder.resource;
   }

   ...
}
```

#### More readings

-   [Double-checked locking in Wikipedia](http://en.wikipedia.org/wiki/Double-checked_locking)
-   [The “Double-Checked Locking is Broken” Declaration](http://www.cs.umd.edu/~pugh/java/memoryModel/DoubleCheckedLocking.html)
-   <http://gee.cs.oswego.edu/dl/cpj/jmm.html> Synchronization and the Java Memory Model\] by Doug Lea
-   “Item 71 : Use lazy initialization judiciously” of Effective Java, 2nd Ed.
-   [How Synchronization works in Java ?](http://javarevisited.blogspot.com/2011/04/synchronization-in-java-synchronized.html) by Javin Paul

#### More code samples

##### `CommonAnnotationBeanPostProcessor.findResourceMetadata` method of Spring framework

The following code from Spring framework uses double-checked locking for non-volatile field.

Is it safe ?

Assuming `ConcurrentHashMap.put` method may not be so complicated to expose stale values to threads and the local codes inside second check block (inner if clause checking `metadata == null`) would not be reordered with `ConcurrentHashMap.put` method execution, the below code could be almost safe ?

``` java
package org.springframework.context.annotation;

public class CommonAnnotationBeanPostProcessor extends ...{

  ...
  private transient final Map<Class<?>, InjectionMetadata> injectionMetadataCache =
  new ConcurrentHashMap<Class<?>, InjectionMetadata>();

  ...

  private InjectionMetadata findResourceMetadata(final Class clazz) {
    // Quick check on the concurrent map first, with minimal locking.
    InjectionMetadata metadata = this.injectionMetadataCache.get(clazz);
    if (metadata == null) {
      synchronized (this.injectionMetadataCache) {
        metadata = this.injectionMetadataCache.get(clazz);
        if (metadata == null) {
          final InjectionMetadata newMetadata = new InjectionMetadata(clazz);
          ReflectionUtils.doWithFields(clazz, new ReflectionUtils.FieldCallback() {
            public void doWith(Field field) {
              if (webServiceRefClass != null
                  && field.isAnnotationPresent(webServiceRefClass)) {
                if (Modifier.isStatic(field.getModifiers())) {
                  throw new IllegalStateException("@WebServiceRef annotation is not supported on static fields");
                }
                newMetadata.addInjectedField(new WebServiceRefElement(field, null));
              }
              else if (ejbRefClass != null
                  && field.isAnnotationPresent(ejbRefClass)) {
                if (Modifier.isStatic(field.getModifiers())) {
                  throw new IllegalStateException("@EJB annotation is not supported on static fields");
                }
                newMetadata.addInjectedField(new EjbRefElement(field, null));
              }
              else if (field.isAnnotationPresent(Resource.class)) {
                if (Modifier.isStatic(field.getModifiers())) {
                  throw new IllegalStateException("@Resource annotation is not supported on static fields");
                }
                if (!ignoredResourceTypes.contains(field.getType().getName())) {
                  newMetadata.addInjectedField(new ResourceElement(field, null));
                }
              }
            }
          });
          ReflectionUtils.doWithMethods(clazz, new ReflectionUtils.MethodCallback() {
            public void doWith(Method method) {
              if (webServiceRefClass != null
                  && method.isAnnotationPresent(webServiceRefClass)
                  &&
                  method.equals(ClassUtils.getMostSpecificMethod(method, clazz))) {
                if (Modifier.isStatic(method.getModifiers())) {
                  throw new IllegalStateException("@WebServiceRef annotation is not supported on static methods");
                }
                if (method.getParameterTypes().length != 1) {
                  throw new IllegalStateException("@WebServiceRef annotation requires a single-arg method: " + method);
                }
                PropertyDescriptor pd = BeanUtils.findPropertyForMethod(method);
                newMetadata.addInjectedMethod(new WebServiceRefElement(method, pd));
              }
              else if (ejbRefClass != null && method.isAnnotationPresent(ejbRefClass) &&
                  method.equals(ClassUtils.getMostSpecificMethod(method, clazz))) {
                if (Modifier.isStatic(method.getModifiers())) {
                  throw new IllegalStateException("@EJB annotation is not supported on static methods");
                }
                if (method.getParameterTypes().length != 1) {
                  throw new IllegalStateException("@EJB annotation requires a single-arg method: " + method);
                }
                PropertyDescriptor pd = BeanUtils.findPropertyForMethod(method);
                newMetadata.addInjectedMethod(new EjbRefElement(method, pd));
              }
              else if (method.isAnnotationPresent(Resource.class) &&
                  method.equals(ClassUtils.getMostSpecificMethod(method, clazz))) {
                if (Modifier.isStatic(method.getModifiers())) {
                  throw new IllegalStateException("@Resource annotation is not supported on static methods");
                }
                Class[] paramTypes = method.getParameterTypes();
                if (paramTypes.length != 1) {
                  throw new IllegalStateException("@Resource annotation requires a single-arg method: " + method);
                }
                if (!ignoredResourceTypes.contains(paramTypes[0].getName())) {
                  PropertyDescriptor pd = BeanUtils.findPropertyForMethod(method);
                  newMetadata.addInjectedMethod(new ResourceElement(method, pd));
                }
              }
            }
          });
          metadata = newMetadata;
          this.injectionMetadataCache.put(clazz, metadata);
        }
      }
    }
    return metadata;
  }

  ...
}
```

### Sample usage of `ConcurrentHashMap`

-   Can use `ConcurrentHashMap` in multi-threaded context without locking
-   Pay attention to [`putIfAbsent`](http://docs.oracle.com/javase/6/docs/api/java/util/concurrent/ConcurrentHashMap.html#putIfAbsent) method.

<!-- -->

-   Sample usage
    -   [`org.apache.commons.lang3.time.FormatCache`](https://github.com/apache/commons-lang/blob/4777c3a5e4291af2420db57d008152c70c4a8f24/src/main/java/org/apache/commons/lang3/time/FormatCache.java#L83)
    -   In the following code, **no** locking is used to avoid concurrent update on cache. The update on the same key is avoided by `putIfAbsent` method. This type of approach that permit the concurrent trial of update is affordable 'cause `format` is somewhat light-weight object. For, more heavy-weight object, it is preferable to prevent the object to cache from being created more than once, by using locking

``` java
abstract class FormatCache<F extends Format> {

    static final int NONE= -1;

    private final ConcurrentMap<MultipartKey, F> cInstanceCache
        = new ConcurrentHashMap<MultipartKey, F>(7);

    private static final ConcurrentMap<MultipartKey, String> cDateTimeInstanceCache
        = new ConcurrentHashMap<MultipartKey, String>(7);

    public F getInstance() {
        return getDateTimeInstance(DateFormat.SHORT, DateFormat.SHORT, TimeZone.getDefault(), Locale.getDefault());
    }

    public F getInstance(final String pattern, TimeZone timeZone, Locale locale) {
        if (pattern == null) {
            throw new NullPointerException("pattern must not be null");
        }
        if (timeZone == null) {
            timeZone = TimeZone.getDefault();
        }
        if (locale == null) {
            locale = Locale.getDefault();
        }
        final MultipartKey key = new MultipartKey(pattern, timeZone, locale);
        F format = cInstanceCache.get(key);
        if (format == null) {
            format = createInstance(pattern, timeZone, locale);
            final F previousValue= cInstanceCache.putIfAbsent(key, format);
            if (previousValue != null) {
                // another thread snuck in and did the same work
                // we should return the instance that is in ConcurrentMap
                format= previousValue;
            }
        }
        return format;
    }

    ...
}
```

### Generic method declaration

-   Case 1 : **The type parameter is used only for method parameters, not for return type.**

&lt;T extends Object&gt; is definitely different from &lt;?&gt;. The former means some fixed type, but the latter means unfixed type. In other words, with &lt;?&gt; the type can vary among executions.

``` java
public static <T> PropertyMeta<?> getPropertyMeta(
      @Nonnull String name, @Nonnull PropertyType type,
      String title, String desc, Boolean required,
      Double max, Boolean exclusiveMax, Double min,
      Doolean exclusiveMin, Integer maxLen, Integer minLen,
      T[] enums, String pattern, T defaultValue){
...
}
```

### Safe release of system resources

Classes in `java.io` or `java.sql` packages handle system resources on networking or file system. When using them, if the resources are not released in some circumstances, it may cause the performance degradation and finally more serious problems. Java has well-shaped code pattern or guideline to prevent these resource leakage.

-   **Coding guideline**
    -   <u>**Process system resources entirely in A local scope**</u>
        -   Do **NOT** define them as a member fields as possible.
        -   Do **NOT** handle an object for a system resource in multiple methods passing them.
        -   In usual coding pattern, `java.sql.Connection` is exceptional.
    -   <u>**Process system resources in `try/catch/finally` statement.**</u>
    -   <u>**Define variables for resource objects before `try` block.**</u>
    -   <u>**Create, assign and use the resource objects IN `try` block**</u>
    -   <u>**Close(release) the resource objects IN `finally` block, NEVER in `try` block.**</u>
        -   Exceptionally when assigning multiple resource objects to a same variable in turn within loop, the resource objects of each iteration should be closed inside the loop (and so in `try` block). - See the 2nd sample below.
    -   <u>**Close all the resource objects, when using multiple ones at the same time.**</u>
    -   <u>**Close the resource objects in order from the underlying one, when the resource objects are in a stack together.**</u>
        -   eg 1. new PrintStream(new BufferedOutputStream(new FileOutputStream(path, true, “utf-8”)));
        -   eg 2. Connection -&gt; Statement -&gt; ResultSet
    -   <u>**AVOID to design the resource class as a parameter type and use the location of the resource as possible**</u>
    -   <u>**The method that access the resource objects handed as a parameter, should NOT release or close the resource.**</u>
        -   It's more intuitive and less confusing, the code block that create the resource object also release it.

<!-- -->

-   Sample 1 - **`java.io`** package

``` java
  public void writeEmployeesToFileMostUnsafe(@Nonnull List<Employee> emps, String path){

    FileOutputStream fos = null;
    BufferedOutputStream bos = null;
    PrintStream ps = null;

    try{
      fos = new FileOutputStream(path);
      bos = new BufferedOutputStream(fos, 1000);
      ps =  new PrintStream(bos, true, "utf-8");

      for(Employee emp : emps){
        ps.println(emp.toString());
      }

      fos.close(); //may not be executed -> Unsafe
      bos.close(); //may not be executed -> Unsafe
      ps.close(); //may not be executed -> Unsafe
    }catch(Exception ex){
      this.logger.error("Can't write the Employee list", ex);
      throw new RuntimeException("Can't write the Employee list", ex);
    }
  }

  public void writeEmployeesToFileUnsafe(@Nonnull List<Employee> emps, String path){

    FileOutputStream fos = null;
    BufferedOutputStream bos = null;
    PrintStream ps = null;

    try{
      fos = new FileOutputStream(path);
      bos = new BufferedOutputStream(fos, 1000);
      ps =  new PrintStream(bos, true, "utf-8"); //may throw UnsupportedEncodingException

      for(Employee emp : emps){
        ps.println(emp.toString());
      }
    }catch(Exception ex){
      this.logger.error("Can't write the Employee list", ex);
      throw new RuntimeException("Can't write the Employee list", ex);
    }finally{
      //This block explicitly closes only ps and does NOT close fos and bos.
      //In usual case, closing ps also closes underlying fos and bos.
      //But if exception occurrs at new PrintStream in try block,
      //bos and fos may remain unclosed.
      // -> UNSAFE

      if(ps != null){
        try{ ps.close(); }
        catch(Exception ex){ this.logger.error("Can't close a resource", ex); }
      }
    }
  }

  public void writeEmployeesToFileAlsoUnsafe(@Nonnull List<Employee> emps, String path){

    PrintStream ps = null;

    try{
      //may throw UnsupportedEncodingException
      ps =  new PrintStream(new BufferedOutputStream(
        new FileOutputStream(path), 1000), true, "utf-8");

      for(Employee emp : emps){
        ps.println(emp.toString());
      }
    }catch(Exception ex){
      this.logger.error("Can't write the Employee list", ex);
      throw new RuntimeException("Can't write the Employee list", ex);
    }finally{
      //Same with right above case.
      //When the exception occurs creating PrintStream after the underlying
      //BufferedStream and FileOutputStream, the underlying streams would
      //remain unclosed with the following code.
      // -> UNSAFE

      if(ps != null){
        try{ ps.close(); }
        catch(Exception ex){ this.logger.error("Can't close a resource", ex); }
      }
    }
  }

  public void writeEmployeesToFileSafe(@Nonnull List<Employee> emps, String path){ // method with SAFE codes

    //Define the resources as local variables as possible.
    //Avoid to define them as member fields, without definite requirement.
    //And do not handle these variables amongst multiple mehtod passing them.
    //In other word, process all the necessary operations inside this method.
    FileOutputStream fos = null;
    BufferedOutputStream bos = null;
    PrintStream ps = null;

    try{
      //There a PrintStream(File) constructor which dosen't
      //need to create underlying FileOutputStream or BufferedOutputStream.
      //But, this sample dosen't use it to show the intended code pattern.

      fos = new FileOutputStream(path);
      bos = new BufferedOutputStream(fos, 1000);
      ps =  new PrintStream(bos, true, "utf-8"); //may throw UnsupportedEncodingException

      for(Employee emp : emps){
        ps.println(emp.toString());
      }

    }catch(Exception ex){
      this.logger.error("Can't write the Employee list", ex);
      throw new RuntimeException("Can't write the Employee list", ex);
    }finally{
      //take care of order - close the underlying reource first.
      //take care of each try/catch pattern
      // - close also can throw exception, but shouldn't block the next tries of close.

      if(fos != null){
        try{ fos.close(); }
        catch(Exception ex){ this.logger.error("Can't close a resource", ex); }
      }
      if(bos != null){
        try{ bos.close(); }
        catch(Exception ex){ this.logger.error("Can't close a resource", ex); }
      }
      if(ps != null){
        try{ ps.close(); }
        catch(Exception ex){ this.logger.error("Can't close a resource", ex); }
      }
    }
  }
```

-   Sample 2 - **`java.sql`** package

``` java
public List<Employee> listEmployees(@Nonnull List<String> ids){  // method with SAFE codes

  if(ids == null) throw new IllegalArgumentException("ID list should be specified.");

  Connection conn = null;
  PreparedStatement ps = null;
  ResultSet rs = null;

  List<Employee> result = new ArrayList<Employee>(ids.size());
  String qry = "SELECT name, age, height, weight, married FROM emp WHERE id = ?";

  Employee emp = null;
  try{
    //In most cases, data-source manages connection pool internally.
    conn = this.getDataSource().getConnection();
    ps = conn.prepareStatement(qry);

    for(String id : ids){
      ps.setString(1, id);
      rs = ps.executeQuery();

      if(rs.next()){
        emp = new Employee();
        emp.setId(id);
        emp.setAge(rs.getInt("age"));
        emp.setHeight(rs.getDouble("height"));
        emp.setWeight(rs.getDouble("weight"));
        emp.setMarried(rs.getBoolean("married"));
        result.add(emp);
      }

      //Close each ResultSet in the iteration
      //Although the spec says that closing of Statement can open only one
      //ResultSet at a time and closing a Statement also close the ResultSet
      //retunred by the Statement, you'd better not trust the implementations
      //too much.
      if(rs != null){
        try{ rs.close(); }
        catch(Exception ex){ this.logger.error("Can't close ResultSet object", ex); }
      }
    }

  }catch(Exception ex){
    this.logger.error("Can't list specified employees.", ex);
    throw new RuntimeException("Can't list specified employees.", ex);
  }finally{
    //take care of order - reverse of creation order
    //take care of each try/catch pattern
    // - close also can throw exception, but shouldn't block the next lines.

    if(rs != null){
      try{ rs.close(); }
      catch(Exception ex){ this.logger.error("Can't close ResultSet object", ex); }
    }
    if(ps != null){
      try{ ps.close(); }
      catch(Exception ex){ this.logger.error("Can't close PreparedStatement object", ex); }
    }
    if(conn != null){
      try{ conn.close(); }
      catch(Exception ex){ this.logger.error("Can't close Connection object", ex); }
    }
  }
  return result;
}
```

-   Sample 3 - **`java.net`** package

``` java
  //method with SAFE codes
  /**
   * Run this method in debug mode, and
   * monitor the conn.connected field (which is protected)
   * and the closed field of underlying input stream object.
   */
  @Test
  public void testUrlConnection() throws Exception{

    // references
    // http://docs.oracle.com/javase/tutorial/networking/urls/readingWriting.html
    // http://docs.oracle.com/javase/6/docs/api/java/net/URLConnection.html
    // http://docs.oracle.com/javase/6/docs/api/java/net/HttpURLConnection.html

    URL url = new URL("http://www.google.com/");
    HttpURLConnection conn = null;
    InputStreamReader isr = null;
    BufferedReader br = null;

    try{
      // "A new connection is opened every time by
      // calling the openConnection method" - from Java SE API
      conn = (HttpURLConnection)(url.openConnection());

      // "Operations that depend on being connected,
      // like getContentLength, will implicitly perform
      // the connection, if necessary."
      // - from Java SE API
      //
      // "The connection is opened implicitly by calling
      // getInputStream." - from The Java Tutorials
      isr = new InputStreamReader(conn.getInputStream());
      br = new BufferedReader(isr);

      String line;
      while((line = br.readLine()) != null){
        System.out.println(line);
      }

    }catch(Exception ex){
      throw ex;
    }finally{

      if(conn != null){
        try{ conn.disconnect(); }
        catch(Exception ex){
          this.logger.error("The connection is not diconnected.", ex);
        }
      }

      // "Invoking the close() methods on the InputStream or
      // OutputStream of an URLConnection after a request may
      // free network resources associated with this instance"
      // - from Java SE API
      if(isr != null){
        try{ isr.close(); }
        catch(Exception ex){
          this.logger.error("The output stream is not closed.", ex);
        }
      }

      if(br != null){
        try{ br.close(); }
        catch(Exception ex){
          this.logger.error("The output stream is not closed.", ex);
        }
      }
    }
  }
```

### Investigating `InitialContext` using recursive call

You can get all name-class pairs under **`InitialContext`** of your application server using the following simple code containing recursive call.

``` java
/**
 * Retrieve all name class pairs recursively under the given naming context.
 *
 * @return
 */
public static Map<String, NameClassPair> getNameClassPairsUnderContext(String name, Context cntx) throws NamingException{

  if(cntx == null) throw new IllegalArgumentException("Context shouldn't be null.");

  Map<String, NameClassPair> result = new HashMap<String, NameClassPair>();
  NamingEnumeration<NameClassPair> pairs = cntx.list("");
  NameClassPair pair = null;
  Object obj = null;
  String path = null;
  while(pairs.hasMore()){
    pair = pairs.next();
    obj = cntx.lookup(pair.getName());
    path = name + "/" + pair.getName();

    if(obj instanceof javax.naming.Context){
      result.put(path, pair);
      result.putAll(getNameClassPairsUnderContext(path, (Context)obj));
    }
    else{ result.put(path, pair);}
  }

  return result;
}
```

### Typical code pattern for Regex API

The simplest code pattern for when a regular expression is used just once is to use **`static Pattern.matches`** method.

``` java
boolean b = Pattern.matches("a*b", "aaaaab");
```

When using a expression several times, it is better to use compiled pattern object considering performance.

``` java
Pattern p = Pattern.compile("a*b");
boolean b1 = p.matcher("aaaaab").matches();
boolean b2 = p.matcher("bbbbb").matches();
```

### Extract RGB pixel data in byte array from general image file

Note that `java.awt.image.BufferedImage.getRGB(int, int, int, int, int[], int, int)` method returns an array of integer pixels in the default RGB color model(`TYPE_INT_ARGB`) regardless of the intrinsic color model. Color conversion takes place if the intrinsic color model is not the default RGB.
By virtue of this automatic conversion inside BufferedImage when necessary, we can easily extract the RGB pixel data of the images in various formats (including those that don't uses RGB color space) using codes like following.

``` java
   public static byte[] extractRgbPixelsFromImage(String path) throws IOException{

      BufferedImage img = ImageIO.read(new File(path));
      int w = img.getWidth();
      int h = img.getHeight();
      int[] pixels = null;

      pixels = img.getRGB(0, 0, w, h, null, 0, w); //ARGB pixels (regardless of original color model and color space)

      byte[] bytes = new byte[w * h * 3];
      int pixel = 0;
      for(int i = 0; i < w * h; i++){
         pixel = pixels[i];
         bytes[i * 3 + 0] = (byte)(pixel >> 16 & 0xff);
         bytes[i * 3 + 1] = (byte)(pixel >> 8 & 0xff);
         bytes[i * 3 + 2] = (byte)(pixel & 0xff);
      }

      return bytes;
   }
```

#### References

-   [`java.awt.image.BufferedImage.getRGB` API](http://docs.oracle.com/javase/7/docs/api/java/awt/image/BufferedImage.html#getRGB(int,%20int,%20int,%20int,%20int%5B%5D,%20int,%20int))
-   [`java.awt.image.PixelGrabber` API](http://docs.oracle.com/javase/7/docs/api/index.html?java/awt/image/PixelGrabber.html)
-   [`javax.imageio` package](http://docs.oracle.com/javase/7/docs/api/index.html?javax/imageio/package-summary.html)
    -   Java Image I/O API(`javax.imageio`) supports `JPEG`, `PNG`, `BMP`, `WBMP`, and `GIF` image formats.
-   [Apache Commons Imaging](https://commons.apache.org/proper/commons-imaging/)
    -   Commons imaging supports `TIFF`, `DCX`, `ICNS`, `ICO/CUR`, `PCX`, and `PNM` image formats.

### Typical usage pattern of SLF4J <strong>`Logger`</strong>

The following code snippet is from the [API documentation of SLF4J <strong>`Logger`</strong> interface](http://www.slf4j.org/apidocs/index.html?org/slf4j/Logger.html). Note that they recommend the logger to be <strong>`final static`</strong> member of the user class.

``` java
 import org.slf4j.Logger;
 import org.slf4j.LoggerFactory;

 public class Wombat {

   final static Logger logger = LoggerFactory.getLogger(Wombat.class);
   Integer t;
   Integer oldT;

   public void setTemperature(Integer temperature) {
     oldT = t;
     t = temperature;
     logger.debug("Temperature set to {}. Old temperature was {}.", t, oldT);
     if(temperature.intValue() > 50) {
       logger.info("Temperature has risen above 50 degrees.");
     }
   }
 }
```

### Simplest Log4j configuration

#### Properties configuration

-   Properties configurations support `DailyRollingFileAppender`, but don't support `TimeBasedRollingPolicy` and `RollingFileAppender`.

``` properties
# Pre-defined appender can be customized using Java system properties : Console, DRFA

# ext properties are expected to be specified as Java system properties in command-line
ext.log.dir=${user.home}
ext.log.file=log4j.log

# global settings
log4j.rootLogger=INFO,Console,DRFA
log4j.threshhold=ALL

# Console appender
log4j.appender.Console=org.apache.log4j.ConsoleAppender
log4j.appender.Console.Target=System.out
log4j.appender.Console.Threshold=WARN
log4j.appender.Console.layout=org.apache.log4j.PatternLayout
log4j.appender.Console.layout.ConversionPattern=%d{yy/MM/dd HH:mm:ss} %-5p %c{2}: %m%n

# Pre-defined daily rolling file appender
log4j.appender.DRFA=org.apache.log4j.DailyRollingFileAppender
log4j.appender.DRFA.Threshold=DEBUG
log4j.appender.DRFA.File=${ext.log.dir}/${ext.log.file}
log4j.appender.DRFA.DatePattern=.yyyy-MM-dd
log4j.appender.DRFA.MaxBackupIndex=30
log4j.appender.DRFA.layout=org.apache.log4j.PatternLayout
log4j.appender.DRFA.layout.ConversionPattern=%d{yy/MM/dd HH:mm:ss} %-5p %c{2} (%F:%M(%L)) - %m%n
```

#### XML configuration

``` xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration>

<log4j:configuration debug="true" threshold="all" xmlns:log4j="http://jakarta.apache.org/log4j/">
  <root>
    <priority value="DEBUG" />
    <appender-ref ref="Console" />
    <appender-ref ref="DRFA" />
  </root>

  <appender name="Console" class="org.apache.log4j.ConsoleAppender">
    <param name="Target" value="System.out" />
    <param name="Threshold" value="TRACE" />
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="[%d{yy/MM/dd,HH:mm:ss}|%-5p|%c{2}] %m(%M{1}:%L)" />
    </layout>
  </appender>

  <appender name="DRFA" class="org.apache.log4j.rolling.RollingFileAppender">
    <param name="MaxBackupIndex" value="30" />
    <rollingPolicy class="org.apache.log4j.rolling.TimeBasedRollingPolicy">
      <param name="FileNamePattern" value="${LOG_FILE_DIR}\\log4j.%d{yyyy-MM-dd}.log" />
    </rollingPolicy>
    <layout class="org.apache.log4j.PatternLayout">
      <param name="ConversionPattern" value="[%d{yy/MM/dd,HH:mm:ss}|%-5p|%c{2}] %m(%M{1}:%L)%n" />
    </layout>
  </appender>

</log4j:configuration>
```

### Simplest Logback configuration

``` xml
<?xml version="1.0"?>
<configuration debug="true" scan="true" scanPeriod="180 seconds">

   <!-- For more on JMX configuration, refer http://logback.qos.ch/manual/jmxConfig.html -->
   <jmxConfigurator/>

   <property name="_LOG_FILE_DIR" value="${log.file.dir:-${LOG_BASE:-${TEMP:-${user.home}}}}" />
   <property name="_LOG_FILE_NAME" value="${log.file.name:-logback}" /> <!-- excluding path and extension -->
   <property name="_LOG_TIMEZONE" value="${log.timezone:-Asia/Seoul}"/>
   <property name="_LOG_PID" value="${log.pid:-false}" />
   <property name="_LOG_LEVEL_ROOT" value="${log.level.root:-${log.level.default:-INFO}}"/>
   <property name="_LOG_LEVEL_SPRING" value="${log.level.spring:-${log.level.default:-INFO}}"/>
   <property name="_LOG_LEVEL_MYBATIS" value="${log.level.mybatis:-${log.level.default:-INFO}}"/>
   <property name="_LOG_LEVEL_JETTY" value="${log.level.jetty:-${log.level.default:-DEBUG}}" />
   <property name="_LOG_LEVEL_AKKA" value="${log.level.akka:-${log.level.default:-DEBUG}}" />
   <property name="_LOG_LEVEL_AKKA_CLUSTER_HEARTBEAT" value="${log.level.akka.clusterHeartBeat:-OFF}"/>

   <if condition='property("_LOG_PID").trim().equals("true")'>
      <then>
         <!-- For more on conversion pattern, refer http://logback.qos.ch/manual/layouts.html#ClassicPatternLayout -->
         <property name="_PATTERN_HEADER" value="[%d{yy/MM/dd HH:mm:ss, ${_LOG_TIMEZONE}}|%-5.-5p|%X{pid}|%-15.-15t|%40.40c{2}]" />
         <!-- %M is not particularly fast. Thus, it should be uses only in test -->
         <property name="_PATTERN_HEADER_METHOD" value="[%d{yy/MM/dd HH:mm:ss, ${_LOG_TIMEZONE}}|%-5.-5p|%X{pid}|%-15.-15t|%40.40c{2}|%M]" />
      </then>
      <else>
         <property name="_PATTERN_HEADER" value="[%d{yy/MM/dd HH:mm:ss, ${_LOG_TIMEZONE}}|%-5.-5p|%-15.-15t|%40.40c{2}]" />
         <property name="_PATTERN_HEADER_METHOD" value="[%d{yy/MM/dd HH:mm:ss, ${_LOG_TIMEZONE}}|%-5.-5p|%-15.-15t|%40.40c{2}|%M]" />
      </else>
   </if>

   <appender name="Console" class="ch.qos.logback.core.ConsoleAppender">
      <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
         <level>DEBUG</level>
      </filter>
      <encoder>
         <pattern>${_PATTERN_HEADER} %m%n</pattern>
      </encoder>
   </appender>
   <!-- For more on RollingFileAppender, refer http://logback.qos.ch/manual/appenders.html#RollingFileAppender -->
   <appender name="DRFA" class="ch.qos.logback.core.rolling.RollingFileAppender">
      <!-- Do NOT specify file element to use prudent mode -->
      <prudent>true</prudent>
      <rollingPolicy class="ch.qos.logback.core.rolling.TimeBasedRollingPolicy">
         <fileNamePattern>${_LOG_FILE_DIR}/${_LOG_FILE_NAME}.%d{yyyy-MM-dd, ${_LOG_TIMEZONE}}.log</fileNamePattern>
         <maxHistory>10</maxHistory>
      </rollingPolicy>
      <filter class="ch.qos.logback.classic.filter.ThresholdFilter">
         <level>DEBUG</level>
      </filter>
      <encoder>
         <pattern>${_PATTERN_HEADER} %m%n</pattern>
      </encoder>
   </appender>

   <root level="${_LOG_LEVEL_ROOT}">
      <appender-ref ref="Console" />
      <appender-ref ref="DRFA" />
   </root>

   <logger name="org.springframework" level="${_LOG_LEVEL_SPRING}" additivity="false">
      <appender-ref ref="Console" />
      <appender-ref ref="DRFA" />
   </logger>

   <logger name="org.apache.ibatis" level="${_LOG_LEVEL_MYBATIS}" additivity="false">
      <appender-ref ref="Console" />
      <appender-ref ref="DRFA" />
   </logger>

  <logger name="org.mybatis.spring" level="${_LOG_LEVEL_MYBATIS}" additivity="false">
      <appender-ref ref="Console" />
      <appender-ref ref="DRFA" />
   </logger>

   <logger name="org.eclipse.jetty" level="${_LOG_LEVEL_JETTY}" additivity="false">
      <appender-ref ref="Console" />
      <appender-ref ref="DRFA" />
   </logger>

   <logger name="org.eclipse.jetty.http.HttpParser" level="${_LOG_LEVEL_JETTY}" additivity="false">
      <appender-ref ref="DRFA" />
   </logger>

   <logger name="org.eclipse.jetty.webapp.WebAppClassLoader" level="${_LOG_LEVEL_JETTY}" additivity="false">
      <appender-ref ref="DRFA" />
   </logger>

   <logger name="akka" level="${_LOG_LEVEL_AKKA}" additivity="false">
      <appender-ref ref="Console" />
      <appender-ref ref="DRFA" />
   </logger>

   <logger name="akka.cluster.ClusterHeartbeatSender" level="${_LOG_LEVEL_AKKA_CLUSTER_HEARTBEAT}" additivity="false">
      <appender-ref ref="DRFA" />
   </logger>
</configuration>
```

### Identifying the implementation of JAXP in use

``` java
      System.out.printf("%1$s = %2$s\n",
            "javax.xml.parsers.SAXParserFactory", System.getProperty("javax.xml.parsers.SAXParserFactory"));
      System.out.printf("%1$s = %2$s\n",
            "javax.xml.parsers.DocumentBuilderFactory", System.getProperty("javax.xml.parsers.DocumentBuilderFactory"));
      System.out.printf("%1$s = %2$s\n",
            "javax.xml.transform.TransformerFactory", System.getProperty("javax.xml.transform.TransformerFactory"));
      System.out.printf("%1$s = %2$s\n",
            "javax.xml.xpath.XPathFactory", System.getProperty("javax.xml.xpath.XPathFactory"));
      System.out.printf("%1$s = %2$s\n",
            "javax.xml.validation.SchemaFactory", System.getProperty("javax.xml.validation.SchemaFactory"));

      System.out.printf("SaxParserFactory implementation = %1$s\n",
            SAXParserFactory.newInstance().getClass().getName());
      System.out.printf("DocumentBuilderFactory implementation = %1$s\n",
            DocumentBuilderFactory.newInstance().getClass().getName());
      System.out.printf("TransformerFactory implementation = %1$s\n",
            TransformerFactory.newInstance().getClass().getName());
      System.out.printf("XPathFactory implementation = %1$s\n",
            XPathFactory.newInstance().getClass().getName());
      System.out.printf("SchemaFactory implementation = %1$s\n",
            SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI).getClass().getName());
```

### Validating XML document using DTD, XML Schema, RELAX NG or Schematron

As of Java SE 1.5, **`javax.xml.validation`** package is added. It makes the validation of XML using various schema language more clear and convenient.

#### Validating with DOM praser explicitly

The following code snippet is a little bit enhancement of the one from [the API documentation of `javax.xml.validation` package](http://docs.oracle.com/javase/6/docs/api/index.html?javax/xml/validation/package-summary.html).

``` java
    DocumentBuilderFactory builderFactory = DocumentBuilderFactory.newInstance();
    builderFactory.setNamespaceAware(true);
    builderFactory.setValidating(false);

    // parse an XML document into a DOM tree
    DocumentBuilder parser = builderFactory .newDocumentBuilder();
    Document document = parser.parse(new File("instance.xml"));

    // create a SchemaFactory capable of understanding WXS schemas
    SchemaFactory factory = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);

    // load a WXS schema, represented by a Schema instance
    Source schemaFile = new StreamSource(new File("mySchema.xsd"));
    Schema schema = factory.newSchema(schemaFile);

    // create a Validator instance, which can be used to validate an instance document
    Validator validator = schema.newValidator();

    // validate the DOM tree
    try {
        validator.validate(new DOMSource(document));
    } catch (SAXException e) {
        // instance document is invalid!
    }
```

#### Validating without parser explicitly

When using DOM parser, the exact location of invalid element is difficulty to get. To get exact location, you should use SAX parser or stream parser. When using stream parser, you may not create explicitly stream parser in your code. Instead, just use

`StreamSource`.

``` java
    ...
    URL schUrl = ClassLoader.getSystemResource("component-meta.xsd");
    URL xmlUrl = ClassLoader.getSystemResource("component-meta-valid.xml");

    SchemaFactory sf = SchemaFactory.newInstance(XMLConstants.W3C_XML_SCHEMA_NS_URI);
    //A Schema object is thread safe and applications are encouraged to share it across many parsers in many threads.
    Schema sch = sf.newSchema(new java.io.File(schUrl.toURI()));
    //A validator object is not thread-safe and not reentrant.
    Validator vldt = sch.newValidator();

    SimpleCollectiveErrorHandler errHandler = new SimpleCollectiveErrorHandler();
    vldt.setErrorHandler(errHandler);
    vldt.validate(new StreamSource(new java.io.File(xmlUrl.toURI())));

    List<SAXParseException> errors = errHandler.getErrors();

    System.err.println("There exist " + errors.size() + " errors.");
    for(SAXParseException error : errors){
      SimpleCollectiveErrorHandler.printSAXParseException(System.err, error);
    }

    ....
```

#### Error Handler

In the above code, a custom `ErrorHandler` is used. The error handler classes provided by JDK is somewhat simple. The source of the custom error handler (**'`SimpleCollectiveErrorHandler`**') is like as below.

``` java
public class SimpleCollectiveErrorHandler extends DefaultHandler{

  private List<SAXParseException> errors = new java.util.ArrayList<SAXParseException>();

  public List<SAXParseException> getErrors(){ return this.errors; }

  public boolean hasError(){ return !this.errors.isEmpty(); }

  @Override public void warning(SAXParseException ex){}
  @Override public void error(SAXParseException ex){ this.errors.add(ex); }
  @Override public void fatalError(SAXParseException ex){ this.errors.add(ex); }

  public void printErrors(PrintStream ps){
    if(ps == null) return;
    ps.println(">> Errors : " + this.errors.size());
    for(SAXParseException ex: this.errors)    printSAXParseException(ps, ex);
    ps.println("");
  }

  public static void printSAXParseException(PrintStream ps, SAXParseException ex){
    ps.println("Line : " + ex.getLineNumber() + ", Column : " + ex.getColumnNumber() + " ; " + ex.getMessage());
  }
}
```

#### Validating XML document with no declared namespace using schema

When validating XML documents with no namespace related attributes such as **`xmlns`** or **`xsi:noNamespaceSchemaLocation`** in the root element, Applying schema with **`xmlns`** or **`targetNamespace`** would cause fundamental errors that say's “Cannot find the declaration of element ...”.

To successfully apply the schema without the modification (such as adding `xmlns` attribute.) of XML documents, the schema to apply should not have `xmlns` and `targetNamespace` attributes.

-   XLM document to validate

``` xml
<?xml version="1.0" encoding="utf-8"?>

<component-meta>
  <name>pie-chart</name>
  <ver>1.0</ver>
  <parts>
    <part>
      <name>pie</name>
      <properties>
        <property>
          <name>title</name>
          ...
```

-   Schema to use

``` xml
<xsd:schema
  xmlns:xsd="http://www.w3.org/2001/XMLSchema"
  elementFormDefault="qualified">

<xsd:annotation>
  <xsd:documentation>
adding the following attribute to the schema element will prevent
applying this schema to the document without namespace declarations
using xmlns or xsi:noNamespaceSchemaLocation attribute

  xmlns="http://www.3rdstage.org/schema/component-meta"
  targetNamespace="http://www.3rdstage.org/schema/component-meta"
  </xsd:documentation>
</xsd:annotation>

<xsd:element name="component-meta" type="component-metaType"/>

<xsd:complexType name="component-metaType">
  <xsd:sequence>
    <xsd:element name="name">
      <xsd:simpleType>
        <xsd:restriction base="xsd:string">
          <xsd:pattern value="[a-zA-Z][-_:.0-9a-zA-Z]*"/>
        </xsd:restriction>
      </xsd:simpleType>
    </xsd:element>
...
```

-   More readings
    -   [How to Validate XML using Java](http://www.edankert.com/validate.html)
    -   [The Java XML Validation API <i>from developerWorks</i>](http://www.ibm.com/developerworks/xml/library/x-javaxmlvalidapi/index.html)(08 Aug 2006)

### Access XML using XPath and DOM

Maybe, the most common use case of XPath would be along with DOM representation of XML. The following sample is provided in the [API documentation of <strong>`javax.xml.xpath`</strong> package](http://docs.oracle.com/javase/6/docs/api/index.html?javax/xml/xpath/package-summary.html).
Note that <strong>`XPathFactory`</strong> and <strong>`XPath`</strong> class is <strong>NOT expected to be thread-safe</strong>, says the API documentation.

``` java
// parse the XML as a W3C Document
DocumentBuilder builder = DocumentBuilderFactory.newInstance().newDocumentBuilder();
Document document = builder.parse(new File("/widgets.xml"));

XPath xpath = XPathFactory.newInstance().newXPath();
String expression = "/widgets/widget";
Node widgetNode = (Node) xpath.evaluate(expression, document, XPathConstants.NODE);
```

### Applying XQuery on HTML

Saxon can't parse HTML file directly. But using TagSoup along with Saxon, you can parse HTML and apply XPath or XQuery to access data. The sample below shows the overall programming.

``` java5
package thirdstage.exercise.xml.saxon;

import java.io.File;
import java.io.FileInputStream;
import java.net.URL;

import javax.xml.parsers.DocumentBuilder;
import javax.xml.transform.Source;
import javax.xml.transform.dom.DOMSource;
import javax.xml.transform.sax.SAXSource;

import net.sf.saxon.Configuration;
import net.sf.saxon.dom.DocumentBuilderImpl;
import net.sf.saxon.lib.ParseOptions;
import net.sf.saxon.lib.Validation;
import net.sf.saxon.s9api.Processor;
import net.sf.saxon.s9api.XQueryCompiler;
import net.sf.saxon.s9api.XQueryEvaluator;
import net.sf.saxon.s9api.XQueryExecutable;
import net.sf.saxon.s9api.XdmNode;
import net.sf.saxon.s9api.XdmValue;

import org.ccil.cowan.tagsoup.Parser;
import org.testng.annotations.Test;
import org.w3c.dom.Document;
import org.xml.sax.InputSource;
import org.xml.sax.XMLReader;

...

  @Test
  public void testXqueryOnHtmlAlongWithTagSoup() throws Exception{

    Configuration cfg = new Configuration();
    cfg.setSchemaValidationMode(Validation.LAX);
    cfg.setValidation(false);
    cfg.setValidationWarnings(true);
    Processor proc = new Processor(cfg);

    URL url = ClassLoader.getSystemResource("thirdstage/exercise/xml/saxon/krx-stock-code-only-10.html");
    XMLReader xr = new org.ccil.cowan.tagsoup.Parser();
    //xr.setFeature(Parser.namespacesFeature, false);
    Source src = new SAXSource(xr, new InputSource(new FileInputStream(new File(url.toURI()))));
    net.sf.saxon.s9api.DocumentBuilder db = proc.newDocumentBuilder();
    XdmNode input = db.build(src);

    String qr = new StringBuilder()
      .append("declare default element namespace \"http://www.w3.org/1999/xhtml\";\n")
      .append("for $x in /html/body[1]/table[1]/tr/td[1]/child::text() return $x").toString();
    XQueryCompiler xqc = proc.newXQueryCompiler();
    XQueryExecutable xqe = xqc.compile(qr);
    XQueryEvaluator xqev = xqe.load();
    xqev.setSource(input.asSource());

    XdmValue result = xqev.evaluate();

    System.out.printf("The result of query contains %1$d items.\n", result.size());
    System.out.printf(result.toString());
  }

...
```

### Marshalling or Unmarshalling XML using JAXB

The main entrance class of JAXB is [**`javax.xml.bind.JAXBContext`**](http://docs.oracle.com/javase/7/docs/api/index.html?javax/xml/bind/JAXBContext.html). Typically, JAXBContext object is created on the behalf of context object(s) and then, marshaller, unmarshaller and validator can be obtained from the context.

Typical code sample from the API document is as following

``` java
   JAXBContext jc = JAXBContext.newInstance( "com.acme.foo" );

   // unmarshal from foo.xml
   Unmarshaller u = jc.createUnmarshaller();
   FooObject fooObj = (FooObject)u.unmarshal( new File( "foo.xml" ) );

   // marshal to System.out
   Marshaller m = jc.createMarshaller();
   m.marshal( fooObj, System.out );
```

API documentation of `Marshaller` and `Unmarshaller` has more sample codes in various circumstances.

-   [**`Unmarshaller`** documentation](http://jaxb.java.net/nonav/2.2.7/docs/api/javax/xml/bind/Unmarshaller.html)
-   [**`Marshaller`** documentation](http://jaxb.java.net/nonav/2.2.7/docs/api/javax/xml/bind/Marshaller.html)

### Serialize objects into JSON strings using Jackson and JAXB annotations

``` java
   private final List<MessierObject> messierCatalog = new ArrayList<MessierObject>();

   private static final ObjectMapper jacksonMapper = new ObjectMapper();

   static{
      jacksonMapper.registerModule(new JaxbAnnotationModule())
      .configure(MapperFeature.AUTO_DETECT_FIELDS, false)
      .configure(MapperFeature.AUTO_DETECT_CREATORS, false)
      .configure(MapperFeature.AUTO_DETECT_GETTERS, true)
      .configure(MapperFeature.AUTO_DETECT_IS_GETTERS, true)
      .configure(MapperFeature.AUTO_DETECT_SETTERS, true);
   }

   @Nonnull
   public List<MessierObject> getMessierCatalog(){
      return this.messierCatalog;
   }

   public String getMessierCatalogInJsonString() throws Exception{
      String result = null;

      List<MessierObject> mos = this.getMessierCatalog();

      Writer wr = new StringWriter();
      jacksonMapper.writeValue(wr, mos);

      return wr.toString();
   }

   @Immutable
   public static class MessierObject{

      private String number;

      private String ngcNumber;

      private String commonName;

      private String type;

      private Double distance;

      private String constellation;

      private Double apparentMagnitude;

      /**
       * @param number
       * @param ngcNumber
       * @param commonName
       * @param type
       * @param distance
       * @param constellation
       * @param apparentMagnitude
       */
      @JsonCreator
      public MessierObject(@JsonProperty("number") String number,
            @JsonProperty("ngc-number") String ngcNumber,
            @JsonProperty("common-name") String commonName,
            @JsonProperty("type") String type,
            @JsonProperty("distance") Double distance,
            @JsonProperty("constellation") String constellation,
            @JsonProperty("apparent-magnitude") Double apparentMagnitude){
         super();
         this.number = number;
         this.ngcNumber = ngcNumber;
         this.commonName = commonName;
         this.type = type;
         this.distance = distance;
         this.constellation = constellation;
         this.apparentMagnitude = apparentMagnitude;
      }

      public String getNumber(){ return number; }

      public String getNgcNumber(){ return ngcNumber;}

      public String getCommonName(){ return commonName; }

      public String getType(){ return type; }

      public Double getDistance(){ return distance; }

      public String getConstellation(){ return constellation; }

      public Double getApparentMagnitude(){ return apparentMagnitude; }
   }
```

-   references
    -   [Using JAXB annotations with Jackson](http://wiki.fasterxml.com/JacksonJAXBAnnotations)
    -   [jackson-module-jaxb-annotations](https://github.com/FasterXML/jackson-module-jaxb-annotations)
    -   Maven artifacts : [com.fasterxml.jackson.module](http://mvnrepository.com/artifact/com.fasterxml.jackson.module)

### Loading FreeMarker template using classpath resource

Locating resources as a classpath resource is very useful, 'cause it makes the code more portable. When loading template files for FreeMarker, you can set the `ClassTemplateLoader` as a `TemplateLoader` using code like the following.

In the following code, the `Schedule.ftl` is expected to be located in some `foo/bar` directory under the root directories of classpath. But it's not clear, whether the template can be loaded when the file is in the archived file or not.

``` java
...
import freemarker.template.Configuration;
import freemarker.template.Template;
...

  //context or configuration for the FreeMaker, thread-safe without modification after initiation.
  Configuration config = new Configuration();
  config.setWhitespaceStripping(true);
  config.setClassForTemplateLoading(this.getClass(), "/");
  BeansWrapper bw = new BeansWrapper();
  bw.setExposeFields(true);
  config.setObjectWrapper(bw);

  //set static methods and enums as shared variables.
  config.setSharedVariable("statics", BeansWrapper.getDefaultInstance().getStaticModels());
  config.setSharedVariable("enums", BeansWrapper.getDefaultInstance().getEnumModels());

  //template, NOT thread-safe
  Template tpl = config.getTemplate("foo/bar/Schedule.ftl");
  StringWriter sw = new StringWriter();

  tpl.process(schedule, sw);
  String result = sw.toString();

...
```

-   more reading
    -   [`ClassPathResource` of Spring framework](http://static.springsource.org/spring/docs/3.2.x/spring-framework-reference/html/resources.html#resources-implementations-classpathresource)
    -   [`Configuration.setClassForTemplateLoading` API](http://freemarker.org/docs/api/freemarker/template/Configuration.html#setClassForTemplateLoading(java.lang.Class,%20java.lang.String))
    -   [`ClassTemplateLoader` API](http://freemarker.org/docs/api/index.html?freemarker/cache/ClassTemplateLoader.html)

AOP
---

### Sample Aspect to trace method call on `RTMPMinaIoHandler` in Red5 client

-   Red5 client is highly concurrent using Apache MINA. So, tracing internal run-time behavior of it is not so trivial. Using AOP can be a little bit better approach.

``` java
import javax.annotation.Nonnull;

import org.aspectj.lang.JoinPoint.EnclosingStaticPart;
import org.aspectj.lang.JoinPoint.StaticPart;
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.slf4j.Marker;
import org.slf4j.MarkerFactory;
import org.apache.commons.lang3.tuple.Pair;
import org.apache.mina.core.service.IoHandler;
import org.apache.mina.core.session.IoSession;
import org.red5.client.net.rtmp.RTMPMinaIoHandler;

/**
 * @author Sangmoon Oh
 * @since 2015-07-21
 */
@Aspect
public class Red5RtmpClientTracingAspect {

  private transient final Logger logger = LoggerFactory.getLogger(this.getClass());

  public final static Marker AOP_MARKER = MarkerFactory.getMarker("AOP");

  /**
   * Traces {@code sessionCreated}, {@code sessionOpened}, {@code sessionClosed}
   * of {@code RTMPMinaIoHandler}.
   *
   * @param joinPt
   * @param enclosingJoinPt
   * @param sess
   * @param msg
   * @see org.red5.client.net.rtmp.RTMPMinaIoHandler#sessionCreated(IoSession)
   * @see org.red5.client.net.rtmp.RTMPMinaIoHandler#sessionOpened(IoSession)
   * @see org.red5.client.net.rtmp.RTMPMinaIoHandler#sessionClosed(IoSession)
   */
  @Before("call(public void IoHandler.message*(..)) && target(RTMPMinaIoHandler) && args(sess, msg)")
  public void beforeCallMinaIoHandlerMessageMethods(final StaticPart joinPt,
    final EnclosingStaticPart enclosingJoinPt, IoSession sess, Object msg){
    Pair<String, String> fromTo = this.getFromToMethods(joinPt, enclosingJoinPt);

    logger.debug(AOP_MARKER, "Method Call: {} -> {}, Session ID: {}, Message Type: {}",
        fromTo.getLeft(), fromTo.getRight(), (sess != null) ? sess.getId() : "",
        (msg != null) ? msg.getClass().getSimpleName() : "null");
  }

  /**
   * Traces {@code messageReceived} and {@code messageSent} methods of
   * {@code RTMPMinaIoHandler} class.
   *
   * @param joinPt
   * @param enclosingJoinPt
   * @param sess
   * @see org.red5.client.net.rtmp.RTMPMinaIoHandler#messageReceived(IoSession, Object)
   * @see org.red5.client.net.rtmp.RTMPMinaIoHandler#messageSent(IoSession, Object)
   */
  @Before("call(public void IoHandler.session*(..)) && target(RTMPMinaIoHandler) && args(sess)")
  public void beforeCallMinaIoHandlerSessionMethods(final StaticPart joinPt,
    final EnclosingStaticPart enclosingJoinPt, IoSession sess){
    Pair<String, String> fromTo = this.getFromToMethods(joinPt, enclosingJoinPt);

    logger.debug(AOP_MARKER, "Method Call: {} -> {}, Session ID: {}",
        fromTo.getLeft(), fromTo.getRight(), (sess != null) ? sess.getId() : "");
  }

  @Nonnull
  private Pair<String, String> getFromToMethods(@Nonnull StaticPart joinPt, @Nonnull EnclosingStaticPart enclosingJoinPt){
    String from = enclosingJoinPt.getSignature().getDeclaringType().getSimpleName() + "."
        + enclosingJoinPt.getSignature().getName();
    String to = joinPt.getSignature().getDeclaringType().getSimpleName() + "."
        + joinPt.getSignature().getName();

    return Pair.of(from, to);
  }
}
```

`aop.xml` for this Aspect is

``` xml
<aspectj>
   <weaver options="-showWeaveInfo">
      <include within="org.red5..*" />
      <include within="org.apache.mina..*"/>
   </weaver>

   <aspects>
      <aspect name="...Red5RtmpClientTracingAspect" />
   </aspects>
</aspectj>
```

Java EE
-------

### Samples of deployment descriptors

#### **`web.xml`** of Servlet 2.5

``` xml
<?xml version="1.0" encoding="utf-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/j2ee/web-app_2_5.xsd"
  version="2.5">
  <display-name>A Simple Application</display-name>
  <context-param>
    <param-name>Webmaster</param-name>
    <param-value>webmaster@mycorp.com</param-value>
  </context-param>
  <servlet>
    <servlet-name>catalog</servlet-name>
    <servlet-class>com.mycorp.CatalogServlet
    </servlet-class>
    <init-param>
      <param-name>catalog</param-name>
      <param-value>Spring</param-value>
    </init-param>
  </servlet>
  <servlet-mapping>
    <servlet-name>catalog</servlet-name>
    <url-pattern>/catalog/*</url-pattern>
  </servlet-mapping>
  <session-config>
    <session-timeout>30</session-timeout>
  </session-config>
  <mime-mapping>
    <extension>pdf</extension>
    <mime-type>application/pdf</mime-type>
  </mime-mapping>
  <welcome-file-list>
    <welcome-file>index.jsp</welcome-file>
    <welcome-file>index.html</welcome-file>
    <welcome-file>index.htm</welcome-file>
  </welcome-file-list>
  <error-page>
    <error-code>404</error-code>
    <location>/404.html</location>
  </error-page>
</web-app>
```

Spring Framework
----------------

### `BeanFactory` and `ApplicationContext` types

-   [`public interface org.springframework.beans.factory.BeanFactory`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/beans/factory/BeanFactory.html)
-   [`public interface org.springframework.beans.factory.ListableBeanFactory extends BeanFactory`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/beans/factory/ListableBeanFactory.html)
-   [`public interface org.springframework.beans.factory.HierarchicalBeanFactory extends BeanFactory`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/beans/factory/HierarchicalBeanFactory.html)
-   [`public interface org.springframework.context.ApplicationContext extends EnvironmentCapable, ListableBeanFactory, HierarchicalBeanFactory, MessageSource, ApplicationEventPublisher, ResourcePatternResolver`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/context/ApplicationContext.html)
-   [`public interface org.springframework.context.ConfigurableApplicationContext extends ApplicationContext, Lifecycle`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/context/ConfigurableApplicationContext.html)

<!-- -->

-   [Package `org.springframework.beans.factory`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/beans/factory/package-summary.html)
-   [Hierarchy For Package `org.springframework.beans.factory`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/beans/factory/package-tree.html)
-   [Package `org.springframework.context`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/context/package-summary.html)
-   [Hierarchy For Package `org.springframework.context`](http://docs.spring.io/spring/docs/3.1.x/javadoc-api/index.html?org/springframework/context/package-tree.html)

### Boilerplate for Spring configuration

#### `spring-base.xml`

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xmlns:aop="http://www.springframework.org/schema/aop" xmlns:context="http://www.springframework.org/schema/context"
  xmlns:tx="http://www.springframework.org/schema/tx" xmlns:task="http://www.springframework.org/schema/task"
  xmlns:lang="http://www.springframework.org/schema/lang" xmlns:util="http://www.springframework.org/schema/util"
  xsi:schemaLocation="
      http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd
      http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop.xsd
      http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context.xsd
      http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx.xsd
      http://www.springframework.org/schema/task http://www.springframework.org/schema/task/spring-task.xsd
      http://www.springframework.org/schema/lang http://www.springframework.org/schema/lang/spring-lang.xsd
      http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util.xsd">

  <description>
    Spring container configuration
  </description>

   <!-- @TODO Use SpEL to be able to specify custom property files using System Property -->
  <util:properties id="config" location="classpath:.../.../.../config.properties" />
   <!-- For more on property-placeholder, refer https://github.com/spring-projects/spring-framework/blob/4.0.x/spring-context/src/main/resources/org/springframework/context/config/spring-context-4.0.xsd#L22 -->
  <context:property-placeholder properties-ref="config"
    system-properties-mode="ENVIRONMENT" local-override="false" ignore-unresolvable="true" />

   <!-- enabling Auto-wiring -->
  <context:annotation-config>
    <description>Activates various annotations to be detected in bean classes such
      as @Required, @Autowired, @Resource or et. al.
    </description>
  </context:annotation-config>

   <!-- enabling Auto-registering -->
   <!-- @Note Do NOT use auto-registering of Spring beans as possible.
   <context:component-scan base-package=""/>
   -->

   <!-- enabling LTW of AspectJ -->
   <!-- @Note Use compile-time weaving as possible.
   <context:load-time-weaver/>
   -->

   <!-- enabling @Async, @Scheduled ... -->
   <!-- For more, refer http://docs.spring.io/spring-framework/docs/4.0.x/spring-framework-reference/html/scheduling.html -->
  <task:annotation-driven />

  <bean id="mbeanServer" class="org.springframework.jmx.support.MBeanServerFactoryBean">
    <property name="locateExistingServerIfPossible" value="true" />
  </bean>

   <!-- Data access and transaction -->
   <!-- @TODO Add alternative usage of DBCP 1.4 in case of Java SE 1.6 -->
  <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" destroy-method="close">
    <description>
      https://commons.apache.org/proper/commons-dbcp/api-2.1/org/apache/commons/dbcp2/BasicDataSource.html
    </description>
    <property name="jmxName" value="${jmx.domain}:type=fabric,name=dataSource" />
    <property name="driverClassName" value="${datasource.jdbcDriver}" />
    <property name="url" value="${datasource.jdbcUrl}" />
    <property name="username" value="${datasource.user}" />
    <property name="password" value="${datasource.password}" />
    <property name="defaultAutoCommit" value="true" />
    <property name="maxTotal" value="${datasource.maxTotal}" />
    <property name="maxIdle" value="${datasource.maxIdle}" />
    <property name="minIdle" value="${datasource.minIdle}" />
    <property name="initialSize" value="${datasource.initSize}" />
  </bean>

  <bean id="transactionManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <description>
      http://docs.spring.io/spring/docs/4.0.x/javadoc-api/org/springframework/jdbc/datasource/DataSourceTransactionManager.html
      http://mybatis.github.io/spring/transactions.html
      Included in spring-jdbc artifact
    </description>
    <property name="dataSource" ref="dataSource" />
    <property name="defaultTimeout" value="120" />
    <property name="nestedTransactionAllowed" value="false" /> <!-- default is true -->
  </bean>

   <!-- @Note Use 'aspectj' mode as possible to enable in-class call and private methods with @Transactional -->
   <!-- For more on @Transactional in 'aspectj' mode, refer https://gerrydevstory.com/2014/06/28/spring-declarative-transaction-management-with-jdk-proxy-vs-aspectj -->
  <tx:annotation-driven transaction-manager="transactionManager" mode="aspectj"
    proxy-target-class="true" />

  <bean id="databaseIdProvider" class="org.apache.ibatis.mapping.VendorDatabaseIdProvider">
    <description>
      DatabaseIdProvider should be set to SqlSessionFactory directly by setDatabaseIdProvider.
      The databaseIdProvider element in the configuration XML will be ignored when the XML is set to
      SqlSessionFactory.

      http://mybatis.github.io/spring/factorybean.html#Properties
      http://mybatis.github.io/mybatis-3/apidocs/reference/org/apache/ibatis/mapping/VendorDatabaseIdProvider.html
    </description>
    <property name="properties">
      <props>
        <prop key="Microsoft SQL Server">ms</prop>
        <prop key="Oracle">oracle</prop>
        <prop key="MySQL">mysql</prop>
      </props>
    </property>
  </bean>

  <bean id="sqlSessionFactory" class="org.mybatis.spring.SqlSessionFactoryBean">
    <description>
      http://mybatis.github.io/spring/factorybean.html
      http://mybatis.github.io/spring/apidocs/reference/org/mybatis/spring/SqlSessionFactoryBean.html
    </description>
    <property name="dataSource" ref="dataSource" />
    <property name="configLocation" value="${mybatis.configLocation}" />
    <property name="databaseIdProvider" ref="databaseIdProvider" />
  </bean>

   <!-- Scanner for Mybatis SQL Mappers -->
  <bean id="sqlMapperScanner" class="org.mybatis.spring.mapper.MapperScannerConfigurer">
    <description>
      http://mybatis.github.io/spring/apidocs/reference/org/mybatis/spring/mapper/MapperScannerConfigurer.html
      This class seems to have problem when using with placeholder.
    </description>
    <property name="processPropertyPlaceHolders" value="true" />
    <property name="basePackage" value="${mybatis.mappers.basePackage}" />
    <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory" />
    <property name="annotationClass" value="org.springframework.stereotype.Repository" />
  </bean>

   <!-- JXM -->
  <beans profile="!production">
    <description>JMX configuration - to disable JMX, use '-Dspring.profiles.active=production'</description>
    <bean id="jmxAttributeSource" class="org.springframework.jmx.export.annotation.AnnotationJmxAttributeSource" />

    <bean id="manualJmxExporter" class="org.springframework.jmx.export.MBeanExporter"
      lazy-init="false">
      <description>
        http://docs.spring.io/spring/docs/4.0.x/javadoc-api/org/springframework/jmx/export/MBeanExporter.html
      </description>
      <property name="beans">
        <map>
          <entry key="${jmx.domain}:type=fabric,name=transactionManager" value-ref="transactionManager" />
          <entry key="${jmx.domain}:type=fabric,name=databaseIdProvider" value-ref="databaseIdProvider" />
          <entry key="${jmx.domain}:type=fabric,name=sqlSessionFactory" value-ref="sqlSessionFactory" />
          <entry key="${jmx.domain}:type=fabric,name=sqlMapperScanner" value-ref="sqlMapperScanner" />
        </map>
      </property>
      <property name="server" ref="mbeanServer" />
    </bean>

    <bean id="autodetectJmxExporter" class="org.springframework.jmx.export.MBeanExporter"
      lazy-init="false">
      <property name="assembler">
        <bean class="org.springframework.jmx.export.assembler.MetadataMBeanInfoAssembler">
          <property name="attributeSource" ref="jmxAttributeSource" />
        </bean>
      </property>
      <property name="namingStrategy">
        <bean class="org.springframework.jmx.export.naming.MetadataNamingStrategy">
          <description>
            http://docs.spring.io/spring/docs/4.0.x/javadoc-api/org/springframework/jmx/export/naming/MetadataNamingStrategy.html
          </description>
          <property name="attributeSource" ref="jmxAttributeSource" />
          <property name="defaultDomain" value="${jmx.domain}" />
        </bean>
      </property>
      <property name="autodetect" value="true" />
      <property name="autodetectModeName" value="AUTODETECT_ASSEMBLER" />
      <property name="excludedBeans">
        <list>
          <value>dataSource</value>
        </list>
      </property>
      <property name="server" ref="mbeanServer" />
    </bean>
  </beans>

</beans>
```

#### `mybatis.xml`

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
    PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

  <!--
  Some top level elements of configuration will be IGNORED
  when configured via org.mybatis.spring.SqlSessionFactoryBean.setConfigLocation
  method.

  Explicitly documented element is 'environments' (which includes 'dataSource'
  and 'transactionManager' elements).
  Other elements that seem to be IGNORED include 'databaseIdProvider'.

  But 'settings', 'typeAliases' and 'mappers' are documented NOT to be ignored.

  For more, refer http://mybatis.github.io/spring/factorybean.html#Properties
  -->


  <!-- http://mybatis.github.io/mybatis-3/configuration.html#settings -->
  <settings>
    <setting name="cacheEnabled" value="true" /> <!-- default : true -->
    <setting name="useColumnLabel" value="true" /> <!-- default : true -->
    <setting name="useGeneratedKeys" value="true" /> <!-- default : false -->
    <setting name="autoMappingUnknownColumnBehavior" value="FAILING"/> <!-- needs MyBatis 3.4.0 or higher -->
    <setting name="defaultExecutorType" value="SIMPLE" /> <!-- SIMPLE(default), REUSE or BATCH -->
    <setting name="mapUnderscoreToCamelCase" value="true" /> <!-- default : false -->
    <setting name="logImpl" value="SLF4J" />
  </settings>

  <typeHandlers>
    <typeHandler handler="com.skcc.nexcore.vas.support.mybatis.BooleanYnHandler"
      javaType="boolean" jdbcType="CHAR"/>
  </typeHandlers>
</configuration>
```

#### `config.properties`

``` properties
jmx.domain = myapp
datasource.jdbcDriver = com.microsoft.sqlserver.jdbc.SQLServerDriver
datasource.jdbcUrl = jdbc:...
datasource.user = SECU
datasource.password = nexcore
datasource.maxTotal = 5
datasource.maxIdle = 5
datasource.minIdle = 0
datasource.initSize = 2
mybatis.configLocation = classpath:.../mybatis.xml
mybatis.mappers.basePackage = myapp.persistence
```

### Check properties of an object using Spring EL

Spring 3.0 introduced powerful expression language ([SpEL](http://static.springsource.org/spring/docs/3.0.x/spring-framework-reference/html/expressions.html)). We can make a simple utility method to check the properties of an object in any type using SpEL

The following is simple sample.

``` java
import org.springframework.expression.ExpressionParser;
import org.springframework.expression.spel.standard.SpelExpressionParser;
import org.springframework.expression.spel.support.StandardEvaluationContext;

public class ObjectInspector {

   // SpelExpressionParser is thread-safe which is explicitly documented in API doc.
   private ExpressionParser expressionParser = new SpelExpressionParser();

   /**
    * Evaluate the given boolean expression against the specified object.
    *
    * This method is thread-safe.
    *
    * @param obj object to check
    * @param booleanExpr boolean expression to evaluate which should be valid with SpEL
    * @param clazz the type of the <code>obj</code>
    */
   public <T> boolean checkBooleanExpression(T obj, String booleanExpr, Class<T> clazz){

      StandardEvaluationContext cntx = new StandardEvaluationContext(obj);
      return this.expressionParser.parseExpression(booleanExpr).getValue(cntx, Boolean.class);
   }
}
```

Note that the method consists of only two line. With the following unit test sample, you can imagine the robustness of the above method.

``` java
public class ObjectInspectorTest {

   private static ObjectInspector inspector;

   @BeforeClass
   public static void setUpBeforeClass() throws Exception {
      inspector = new ObjectInspector();
   }

   @Test
   public void testCheckBooleanExpression(){

      Person p1 = new Person("Peter", Person.Gender.MALE, (new GregorianCalendar(71, 10, 23)).getTime());
      Person p2 = new Person("Jane", Person.Gender.FEMALE, (new GregorianCalendar(74, 5, 9)).getTime());

      //first eye-checking
      System.out.println(p1.toString());
      System.out.println(p2.toString());

      Assert.assertTrue(inspector.checkBooleanExpression(p1, "'Peter'.equals(#root.name)", Person.class));
      Assert.assertTrue(inspector.checkBooleanExpression(p2, "#root.birthday.month == 5", Person.class));

      Rectangle rct1 = new Rectangle(5, 10, 3, 6);

      Assert.assertTrue(inspector.checkBooleanExpression(rct1, "#root.width == 5", Rectangle.class));
      Assert.assertFalse(inspector.checkBooleanExpression(rct1, "#root.height == 5", Rectangle.class));
      Assert.assertTrue(inspector.checkBooleanExpression(rct1, "#root.area == 15", Rectangle.class));
   }
}

class Person{

   enum Gender{ MALE, FEMALE }

   private String name;
   private Gender gender;
   private Date birthday;

   public Person(String name, Gender gender, Date date){
      this.name = name;
      this.gender = gender;
      this.birthday = date;
   }

   public String getName() { return name; }
   public Gender getGender() { return gender; }
   public Date getBirthday() { return birthday; }

   @Override
   public String toString(){
      return String.format("Name : %1$s, Gender : %2$s, Birthday : %3$tF", this.name, this.gender, this.birthday);
   }
}


class Rectangle{

   private int x1;
   private int x2;
   private int y1;
   private int y2;

   public Rectangle(int x1, int x2, int y1, int y2){
      if(x1 > x2) throw new IllegalArgumentException("x1 should not greater than x2");
      if(y1 > y2) throw new IllegalArgumentException("y1 should not greater than y2");

      this.x1 = x1;
      this.x2 = x2;
      this.y1 = y1;
      this.y2 = y2;
   }

   public int getX1() { return x1; }
   public int getX2() { return x2; }
   public int getY1() { return y1; }
   public int getY2() { return y2; }

   public int getWidth(){ return (x2 - x1); }
   public int getHeight(){ return (y2 - y1); }
   public int getArea(){ return (this.getWidth())*(this.getHeight()); }
}
```

### Mixing Spring property placeholder configurer and SpEL

-   Properties file : **`props-server.properties`**

``` properties
server.threadpool.mbeanName="bean:name=threadPool2"
server.threadpool.corePoolSize=T(Math).min(T(Runtime).getRuntime().availableProcessors(), 6)
server.threadpool.maxPoolSize=10
server.threadpool.queueCapacity=25
server.threadpool.keepAliveSeconds=10
server.threadpool.threadNamePrefix="toolServerThread-"
```

-   Spring configuration file : **`spring-context.xml`**

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
 xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xmlns:context="http://www.springframework.org/schema/context"
 xmlns:util="http://www.springframework.org/schema/util"
 xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-3.0.xsd
  http://www.springframework.org/schema/util http://www.springframework.org/schema/util/spring-util-3.0.xsd
  http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-3.0.xsd">

 <context:property-placeholder location="classpath:sample/server/case2/props-server.properties"/>

 <bean id="jmxExporter"
  class="org.springframework.jmx.export.MBeanExporter" lazy-init="false">
  <property name="beans">
   <map>
    <entry key="#{${server.threadpool.mbeanName}}" value-ref="threadPool"/>
   </map>
  </property>
 </bean>

 <bean id="threadPool"
  class="org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor"
  lazy-init="false">
  <property name="corePoolSize" value="#{${server.threadpool.corePoolSize}}"/>
  <property name="maxPoolSize" value="#{${server.threadpool.maxPoolSize}}"/>
  <property name="threadNamePrefix" value="#{${server.threadpool.threadNamePrefix}}"/>
 </bean>
</beans>
```

MyBatis
-------

### Changing `EntityResolver` for mapper XML

1.  Create **custom DTD** extending [`http://mybatis.org/dtd/mybatis-3-mapper.dtd`](http://mybatis.org/dtd/mybatis-3-mapper.dtd) and locate it in a JAR file
2.  Create **custom `EntityResolver`** extending [`org.apache.ibatis.builder.xml.XMLMapperEntityResolver`](http://mybatis.github.io/mybatis-3/xref/org/apache/ibatis/builder/xml/XMLMapperEntityResolver.html)
3.  Create **custom `XMLConfigureBuilder`** extending `org.apache.ibatis.builder.xml.XMLConfigBuilder` and **custom `XMLMapperBuilder`** extending `org.apache.ibatis.builder.xml.XMLMapperBuilder`
4.  Create **custom `SqlSessionFactoryBuilder`** extending `org.apache.ibatis.session.SqlSessionFactoryBuilder`
5.  Create **custom `SqlSessionFactoryBean`** extending `org.mybatis.spring.SqlSessionFactoryBean`

-   Consideration
    -   `org.apache.ibatis.scripting.xmltags.XMLLanguageDriver`
    -   `org.apache.ibatis.builder.annotation.MapperAnnotationBuilder`
    -   `org.apache.ibatis.scripting.defaults.RawLanguageDriver`

### Using OGNL expressions within SQL statement of MyBatis

You can use OGNL expressions with MyBatis using **`bind`** element.
For more, refer <http://www.mybatis.org/mybatis-3/dynamic-sql.html#bind>

``` xml
   <insert id="insertVisitor" parameterType="face.domain.Visitor"
      useGeneratedKeys="true" keyProperty="visitor.id" keyColumn="id">
      <bind name="isValid" value="(visitor.isValid())? 'Y' : 'N'"/>
insert vas_visitor (
 name, is_valid, valid_from, valid_to, pass_no,
 company, tel_no, prsnnl_id, remarks,
 last_update_by, last_update_at
)values(
 #{visitor.name}, #{isValid}, #{visitor.validFrom}, #{visitor.validTo}, #{visitor.passNo},
 #{visitor.company}, #{visitor.telephone}, #{visitor.personnelId}, #{visitor.remarks},
 'appl', replace(replace(replace(convert(varchar(19),getdate(),120),'-',''),':',''),' ','')
);
   </insert>
```

### Custom MyBatis type handler sample

Most of databases don't support intrinsic `boolean` datatype. JDBC supports implicit and automatic conversion between `true/false` and `'1'/'0'` or `1/0`.

But `'1'/'0'` is not so intuitive and other value pairs such as `'Y'/'N'`, `'y'/'n'`, `'YES'/'NO'`, `'T'/'F'`, or `'true'/'false'` would be preferred. Even the combination of those pairs could be wanted for liberated people. The following type handler can be used for this purpose.

-   References
    -   [`java.sql.ResultSet.getBoolean(String)` API](http://docs.oracle.com/javase/7/docs/api/java/sql/ResultSet.html#getBoolean(java.lang.String))
    -   [The source of `BooleanTypeHandler` built-in MyBatis](https://github.com/mybatis/mybatis-3/blob/master/src/main/java/org/apache/ibatis/type/BooleanTypeHandler.java)

``` java
/**
 * Maps automatically {@code boolean} type property in Java object and string type column in database.
 * <p>
 * The column handled by this class is expected to be {@code CHAR} or {@code VACHAR} datatype.
 * <p>
 * The mapping for {@code NULL} value in database can be configured in constructor.
 *
 * @see <code>https://github.com/mybatis/mybatis-3/blob/master/src/main/java/org/apache/ibatis/type/BooleanTypeHandler.java</code>
 */
public abstract class BooleanStringHandler extends BaseTypeHandler<Boolean>{

   private final org.slf4j.Logger logger = LoggerFactory.getLogger(this.getClass());

   /**
    * the list strings that represent {@code true}
    */
   private final List<String> trueValues = new ArrayList<String>();

   /**
    * the list strings that represent {@code false}
    */
   private final List<String> falseValues = new ArrayList<String>();

   /**
    * determines whether {@code NULL} value in database will be mapped to {@code false} or not.
    */
   private final boolean isNullFalse;

   /**
    * At least one string value should be given for each list of {@code true} values and
    * list of {@code false} values.
    * <p>
    * If more than two values are specified, the first value in either list will be used
    * when inserting or updating the database column.
    *
    * @param trueValues
    * @param falseValues
    */
   public BooleanStringHandler(@NotEmpty String[] trueValues, @NotEmpty String[] falseValues, boolean isNullFalse){
      Validate.isTrue(!ArrayUtils.isEmpty(trueValues),
         "At least one string for true should included.");
      Validate.isTrue(!ArrayUtils.isEmpty(falseValues),
         "At least one string for false should included.");

      for(int i = 0, n = trueValues.length; i < n; i++){ this.trueValues.add(trueValues[i]); }
      for(int i = 0, n = falseValues.length; i < n; i++){ this.falseValues.add(falseValues[i]); }
      this.isNullFalse = isNullFalse;
   }

   @Override
   public void setNonNullParameter(PreparedStatement ps, int i,
      Boolean parameter, JdbcType jdbcType) throws SQLException{

      if(parameter){ ps.setString(i, this.trueValues.get(0)); }
      else{ ps.setString(i, this.falseValues.get(0)); }
   }

   @Override
   public Boolean getNullableResult(ResultSet rs, String columnName)
      throws SQLException{

      String value = rs.getString(columnName);
      if(value == null){ return !isNullFalse; }
      else{ return this.getBooleanFromStringValue(value); }
   }

   @Override
   public Boolean getNullableResult(ResultSet rs, int columnIndex)
      throws SQLException{

      String value = rs.getString(columnIndex);
      if(value == null){ return !isNullFalse; }
      else{ return this.getBooleanFromStringValue(value); }
   }

   @Override
   public Boolean getNullableResult(CallableStatement cs, int columnIndex)
      throws SQLException{

      String value = cs.getString(columnIndex);
      if(value == null){ return !isNullFalse; }
      else{ return this.getBooleanFromStringValue(value); }
   }

   @Nonnull
   private Boolean getBooleanFromStringValue(String value){
      if(this.trueValues.contains(value)){ return Boolean.TRUE; }
      else if(this.falseValues.contains(value)){ return Boolean.FALSE; }
      else{
         this.logger.error("The value('{}') in database is not among expected values.", value);
         throw new IllegalStateException("The value in database is not among expected values.");
      }
   }
}
```

The conversion which permits only `'Y'/'N'` pair (simplest except `'1'/'0'`) can be handled by the following one. Note that relational database world prefers short(but intuitive enough) and capitalized value for enumerations

``` java
/**
 * Maps automatically {@code boolean} type property in Java object and 'Y' or 'N' value in database column.
 * <p>
 * 'Y' or 'N' value in database will be converted to {@code true} or {@code false} value in Java
 * object and vice versa.
 * <p>
 * {@code NULL} value in database will be converted to {@code false}.
 *
 */
public final class BooleanYnHandler extends BooleanStringHandler{

   public BooleanYnHandler(){
      super(new String[]{"Y"}, new String[]{"N"}, true);

   }
}
```

To use the above handler, it should be registered MyBatis configuration file

``` xml
  <typeHandlers>
    <typeHandler handler="support.mybatis.BooleanYnHandler" javaType="boolean" jdbcType="CHAR"/>
  </typeHandlers>
```

When the `Member` class has `boolean isActive` filed and the corresponding column is `CHAR(1) TB_MEMBER.IS_ACTIVE` in database. MyBatis SQL mapper would like the following.

``` xml
<insert id="insertMember" resultType="Member">
insert tb_member (id, name, is_active, registered_at)
values (#{member.id}, #{member.isActive,javaType=boolean,jdbcType=CHAR}, #{member.registeredAt})
</insert>

<select id="selectMemberById">
select id, name, is_active as isActive, registered_at as registerAt
from tb_member
where id = #{id}
</select>
```

ActiveMQ
--------

### Loading embedded ActiveMQ broker and web console respectively

#### Maven depedency configuration

#### Broker

#### Web console

Jetty
-----

### Loading embedded Jetty 9 with JSP and JSTL support

The dependent libraries to enable JSP and JSTL support for Jetty are much various according to the versions or modes of Jetty. There are `org.eclipse.jetty:apache-jsp`, `org.eclipse.jetty:apache-jstl`, `org.apache.taglibs:taglibs-standard-spec`, `org.apache.taglibs:taglibs-standard-impl` or much more modules.

But these make there can be too many possible configurations and make it difficult to setup correct configuration.

As of Jetty 9, the most simple configuration is using only **`org.eclipse.jetty:jetty-jsp`** with no other JSP/JSTL related modules.

``` xml
      ...
      <dependency>
         <groupId>org.eclipse.jetty</groupId>
         <artifactId>jetty-server</artifactId>
         <version>${jetty.version}</version>
      </dependency>
      <dependency>
         <groupId>org.eclipse.jetty</groupId>
         <artifactId>jetty-webapp</artifactId>
         <version>${jetty.version}</version>
      </dependency>
      <dependency>
         <groupId>org.eclipse.jetty</groupId>
         <artifactId>jetty-jsp</artifactId>
         <version>${jetty.version}</version>
      </dependency>
      <dependency>
         <groupId>org.eclipse.jetty</groupId>
         <artifactId>jetty-annotations</artifactId>
         <version>${jetty.version}</version>
      </dependency>
      <dependency>
         <groupId>org.eclipse.jetty</groupId>
         <artifactId>jetty-jmx</artifactId>
         <version>${jetty.version}</version>
      </dependency>
      <dependency>
         <groupId>org.eclipse.jetty</groupId>
         <artifactId>jetty-deploy</artifactId>
         <version>${jetty.version}</version>
      </dependency>
      ...
```

The Java code to setup and load Jetty without `jetty.xml` is like the following.

``` java
      ...
   //final InetAddress addr = InetAddress.getLocalHost();
   final InetAddress addr = InetAddress.getLoopbackAddress();
   final int port = 8080;
   final Server jetty = new Server(new InetSocketAddress(addr, port));
   jetty.addBean(new MBeanContainer(ManagementFactory.getPlatformMBeanServer()));
   Configuration.ClassList.serverDefault(jetty).addBefore("org.eclipse.jetty.webapp.JettyWebXmlConfiguration"
         , "org.eclipse.jetty.annotations.AnnotationConfiguration");
   final String webAppPath = System.getenv("TEMP") + "/activemq-web-console.war";
   final WebAppContext webApp = new WebAppContext(webAppPath, "/admin");
   webApp.setExtractWAR(true);
   webApp.setLogUrlOnStart(true);
   webApp.setAttribute("org.eclipse.jetty.server.webapp.ContainerIncludeJarPattern",
         ".*/[^/]*servlet-api-[^/]*\\.jar$|.*/javax.servlet.jsp.jstl-.*\\.jar$|.*/[^/]*taglibs.*\\.jar$" );
   jetty.setHandler(webApp);
   jetty.setStopAtShutdown(true);
   jetty.start();
   //jetty.join();
   ...
```

-   Readings
    -   [How do you get embedded Jetty 9 to successfully resolve the JSTL URI?](http://stackoverflow.com/questions/17685330/how-do-you-get-embedded-jetty-9-to-successfully-resolve-the-jstl-uri)(Jul 16 '13)
    -   [Example: Embedded Jetty w/ JSP Support](https://github.com/jetty-project/embedded-jetty-jsp)

Maven
-----

### Simple POM

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
   <modelVersion>4.0.0</modelVersion>
   <groupId>thirdstage.exercise</groupId>
   <artifactId>solidity</artifactId>
   <version>0.0.1-SNAPSHOT</version>

   <prerequisites>
      <maven>3.0</maven>
   </prerequisites>

   <properties>
      <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
      <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
      <skipTests>false</skipTests>
      <maven.deploy.skip>true</maven.deploy.skip>
      <maven.javadoc.skip>false</maven.javadoc.skip>
      <maven.site.deploy.skip>true</maven.site.deploy.skip>
      <findbugs.skip>true</findbugs.skip>
      <checkstyle.skip>true</checkstyle.skip>
      <java.version>1.7</java.version>
      <slf4j.version>1.7.21</slf4j.version>
      <logback.version>1.1.7</logback.version>
      <junit.version>4.8.2</junit.version>
      <testng.version>6.9.10</testng.version>
      <commons.lang3.version>3.4</commons.lang3.version>
      <spring.version>4.0.9.RELEASE</spring.version>
      <mybatis.version>3.4.0</mybatis.version>
      <jackson.version>2.7.4</jackson.version>
   </properties>

   <repositories>
      <repository>
         <snapshots>
            <enabled>false</enabled>
         </snapshots>
         <id>central2</id>
         <url>http://repo2.maven.org/maven2/</url>
      </repository>
      <repository>
         <snapshots>
            <enabled>true</enabled>
         </snapshots>
         <id>java.net.public</id>
         <url>https://maven.java.net/content/groups/public/</url>
      </repository>
   </repositories>

   <pluginRepositories>
      <pluginRepository>
         <snapshots>
            <enabled>false</enabled>
         </snapshots>
         <id>central2</id>
         <url>http://repo2.maven.org/maven2/</url>
      </pluginRepository>
   </pluginRepositories>

   <reporting>
      <plugins>
      </plugins>
   </reporting>

   <build>
      <plugins>
         <plugin>
            <groupId>org.basepom.maven</groupId>
            <artifactId>duplicate-finder-maven-plugin</artifactId>
            <executions>
               <execution>
                  <id>find-duplicate-classes</id>
                  <phase>prepare-package</phase>
                  <goals>
                     <goal>check</goal>
                  </goals>
               </execution>
            </executions>
            <configuration>
               <!-- For more, refer https://github.com/basepom/duplicate-finder-maven-plugin/wiki -->
               <skip>false</skip>
               <checkCompileClasspath>false</checkCompileClasspath>
               <checkRuntimeClasspath>true</checkRuntimeClasspath>
               <checkTestClasspath>false</checkTestClasspath>
               <ignoredResourcePatterns>
                  <ignoredResourcePattern>about.html</ignoredResourcePattern>
               </ignoredResourcePatterns>
               <ignoredDependencies>
                  <dependency>
                     <groupId>org.slf4j</groupId>
                     <artifactId>jcl-over-slf4j</artifactId>
                  </dependency>
               </ignoredDependencies>
            </configuration>
         </plugin>
      </plugins>
      <pluginManagement>
         <plugins>
            <!-- core -->
            <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-compiler-plugin</artifactId>
               <version>2.3.2</version>
               <inherited>true</inherited>
               <configuration>
                  <source>${java.version}</source>
                  <target>${java.version}</target>
               </configuration>
            </plugin>
            <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-site-plugin</artifactId>
               <version>3.0</version>
               <dependencies>
                  <dependency>
                     <groupId>org.apache.maven.wagon</groupId>
                     <artifactId>wagon-ssh</artifactId>
                     <version>2.0</version>
                  </dependency>
               </dependencies>
            </plugin>
            <!-- packaging -->
            <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-jar-plugin</artifactId>
               <version>2.3.2</version>
               <configuration>
                  <!-- For more on Maven archiver, refer http://maven.apache.org/shared/maven-archiver/index.html -->
                  <archive>
                     <addMavenDescriptor>false</addMavenDescriptor>
                     <forced>true</forced>
                     <index>true</index>
                     <manifest>
                        <addClasspath>false</addClasspath>
                        <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
                        <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
                        <addExtensions>false</addExtensions>
                        <classpathLayoutType>simple</classpathLayoutType>
                     </manifest>
                     <manifestEntries>
                        <Source-Revision>${project.svn.revision}</Source-Revision>
                     </manifestEntries>
                  </archive>
               </configuration>
            </plugin>
            <!-- reporting -->
            <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-javadoc-plugin</artifactId>
               <version>2.8</version>
               <configuration>
                  <additionalJOptions>
                     <additionalJOption>-Xms128m</additionalJOption>
                  </additionalJOptions>
                  <docencoding>${project.reporting.outputEncoding}</docencoding>
                  <encoding>${project.build.sourceEncoding}</encoding>
                  <doctitle>${project.name} ${project.version} API</doctitle>
                  <windowtitle>${project.name} ${project.version} API</windowtitle>
                  <links>
                     <link>http://docs.oracle.com/javase/7/docs/api/</link>
                     <link>http://docs.oracle.com/javaee/6/api/</link>
                     <link>http://jsr-305.googlecode.com/svn/trunk/javadoc/</link>
                     <link>http://docs.jboss.org/hibernate/beanvalidation/spec/1.1/api/</link>
                     <link>http://docs.jboss.org/hibernate/validator/5.2/api/</link>
                     <link>http://commons.apache.org/proper/commons-lang/javadocs/api-3.3.2/</link>
                     <link>http://commons.apache.org/proper/commons-collections/javadocs/api-release/</link>
                     <link>http://docs.spring.io/spring/docs/4.0.x/javadoc-api/</link>
                  </links>
                  <show>projected</show>
                  <splitindex>true</splitindex>
               </configuration>
            </plugin>
            <plugin>
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-checkstyle-plugin</artifactId>
               <version>2.7</version>
            </plugin>
            <plugin>
               <groupId>org.basepom.maven</groupId>
               <artifactId>duplicate-finder-maven-plugin</artifactId>
               <version>1.2.1</version>
            </plugin>
            <!-- tools supporting -->
            <plugin>
               <!-- http://maven.apache.org/plugins/maven-eclipse-plugin/eclipse-mojo.html -->
               <groupId>org.apache.maven.plugins</groupId>
               <artifactId>maven-eclipse-plugin</artifactId>
               <version>2.9</version>
               <configuration>
                  <!-- the next two item doesn't work on m2e. m2e has its own confgiruation in Eclipse preferences -->
                  <downloadJavadocs>true</downloadJavadocs>
                  <downloadSources>true</downloadSources>
                  <forceRecheck>false</forceRecheck>
               </configuration>
            </plugin>
            <plugin>
               <groupId>org.eclipse.m2e</groupId>
               <artifactId>lifecycle-mapping</artifactId>
               <version>1.0.0</version>
               <configuration>
                  <lifecycleMappingMetadata>
                     <pluginExecutions>
                        <pluginExecution>
                           <pluginExecutionFilter>
                              <groupId>org.apache.maven.plugins</groupId>
                              <artifactId>maven-dependency-plugin</artifactId>
                              <versionRange>[1.0.0,)</versionRange>
                              <goals>
                                 <goal>copy-dependencies</goal>
                              </goals>
                           </pluginExecutionFilter>
                           <action>
                              <ignore />
                           </action>
                        </pluginExecution>
                        <pluginExecution>
                           <pluginExecutionFilter>
                              <groupId>org.apache.maven.plugins</groupId>
                              <artifactId>maven-antrun-plugin</artifactId>
                              <versionRange>[1.0.0,)</versionRange>
                              <goals>
                                 <goal>run</goal>
                              </goals>
                           </pluginExecutionFilter>
                           <action>
                              <ignore />
                           </action>
                        </pluginExecution>
                        <pluginExecution>
                           <pluginExecutionFilter>
                              <groupId>org.codehaus.mojo</groupId>
                              <artifactId>aspectj-maven-plugin</artifactId>
                              <versionRange>[1.0.0,)</versionRange>
                              <goals>
                                 <goal>compile</goal>
                                 <goal>test-compile</goal>
                              </goals>
                           </pluginExecutionFilter>
                           <action>
                              <ignore />
                           </action>
                        </pluginExecution>
                        <pluginExecution>
                           <pluginExecutionFilter>
                              <groupId>org.codehaus.mojo</groupId>
                              <artifactId>build-helper-maven-plugin</artifactId>
                              <versionRange>[1.0.0,)</versionRange>
                              <goals>
                                 <goal>parse-version</goal>
                              </goals>
                           </pluginExecutionFilter>
                           <action>
                              <ignore />
                           </action>
                        </pluginExecution>
                        <pluginExecution>
                           <pluginExecutionFilter>
                              <groupId>net.alchim31.maven</groupId>
                              <artifactId>scala-maven-plugin</artifactId>
                              <versionRange>[1.0.0,)</versionRange>
                              <goals>
                                 <goal>add-source</goal>
                                 <goal>compile</goal>
                                 <goal>testCompile</goal>
                              </goals>
                           </pluginExecutionFilter>
                           <action>
                              <ignore />
                           </action>
                        </pluginExecution>
                     </pluginExecutions>
                  </lifecycleMappingMetadata>
               </configuration>
            </plugin>
         </plugins>
      </pluginManagement>
   </build>

   <dependencies>
      <dependency>
         <groupId>com.google.code.findbugs</groupId>
         <artifactId>jsr305</artifactId>
      </dependency>
      <dependency>
         <groupId>javax.inject</groupId>
         <artifactId>javax.inject</artifactId>
      </dependency>
      <dependency>
         <groupId>javax.validation</groupId>
         <artifactId>validation-api</artifactId>
      </dependency>
      <dependency>
         <groupId>org.hibernate</groupId>
         <artifactId>hibernate-validator</artifactId>
      </dependency>
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>slf4j-api</artifactId>
      </dependency>
      <dependency>
         <groupId>org.slf4j</groupId>
         <artifactId>jcl-over-slf4j</artifactId>
      </dependency>
      <dependency>
         <groupId>ch.qos.logback</groupId>
         <artifactId>logback-classic</artifactId>
      </dependency>
      <dependency>
         <groupId>junit</groupId>
         <artifactId>junit</artifactId>
      </dependency>
      <dependency>
         <groupId>org.testng</groupId>
         <artifactId>testng</artifactId>
      </dependency>
      <dependency>
         <groupId>org.apache.commons</groupId>
         <artifactId>commons-lang3</artifactId>
      </dependency>
   </dependencies>
   <dependencyManagement>
      <dependencies>
         <dependency>
            <!-- JSR 305 annotations which includes also JCIP annotations -->
            <groupId>com.google.code.findbugs</groupId>
            <artifactId>jsr305</artifactId>
            <version>2.0.3</version>
         </dependency>
         <dependency>
            <!-- JSR 330 annotations -->
            <groupId>javax.inject</groupId>
            <artifactId>javax.inject</artifactId>
            <version>1</version>
         </dependency>
         <dependency>
            <!-- JSR 349 annotations -->
            <groupId>javax.validation</groupId>
            <artifactId>validation-api</artifactId>
            <version>1.1.0.Final</version>
         </dependency>
         <dependency>
            <!-- Hibernate Validator -->
            <groupId>org.hibernate</groupId>
            <artifactId>hibernate-validator</artifactId>
            <version>5.2.4.Final</version>
         </dependency>
         <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>slf4j-api</artifactId>
            <version>${slf4j.version}</version>
         </dependency>
         <dependency>
            <groupId>org.slf4j</groupId>
            <artifactId>jcl-over-slf4j</artifactId>
            <version>${slf4j.version}</version>
         </dependency>
         <dependency>
            <groupId>ch.qos.logback</groupId>
            <artifactId>logback-classic</artifactId>
            <version>${logback.version}</version>
         </dependency>
         <dependency>
            <groupId>org.codehaus.janino</groupId>
            <artifactId>janino</artifactId>
            <version>2.7.8</version>
         </dependency>
         <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
            <scope>test</scope>
         </dependency>
         <dependency>
            <groupId>org.testng</groupId>
            <artifactId>testng</artifactId>
            <version>${testng.version}</version>
            <scope>test</scope>
         </dependency>
         <dependency>
            <groupId>org.mockito</groupId>
            <artifactId>mockito-core</artifactId>
            <version>${mockito.version}</version>
            <scope>test</scope>
         </dependency>
         <dependency>
            <groupId>org.apache.commons</groupId>
            <artifactId>commons-lang3</artifactId>
            <version>${commons.lang3.version}</version>
         </dependency>
      </dependencies>
   </dependencyManagement>
</project>
```

### Typical sample of parent POM

``` xml
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">

  <!-- For more on Maven project descriptor, refer http://maven.apache.org/ref/2.2.1/maven-model/maven.html -->
  <modelVersion>4.0.0</modelVersion>
  <groupId>thridstage</groupId>
  <artifactId>thridstage-framework-parent</artifactId>
  <version>3.0.0-SNAPSHOT</version>
  <packaging>pom</packaging>

  <name>Thirdstage Framework</name>
  <url>...</url>
  <inceptionYear>2012</inceptionYear>
  <organization>
    <name>...</name>
    <url>...</url>
  </organization>

  <prerequisites>
    <maven>3.0</maven>
  </prerequisites>

  <properties>
    <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
    <project.reporting.outputEncoding>UTF-8</project.reporting.outputEncoding>
    <graphviz.home>${env.GRAPHVIZ_HOME}</graphviz.home>
    <skipTests>false</skipTests>
    <maven.deploy.skip>true</maven.deploy.skip>
    <maven.javadoc.skip>false</maven.javadoc.skip>
    <maven.site.deploy.skip>true</maven.site.deploy.skip>
    <findbugs.skip>true</findbugs.skip>
    <checkstyle.skip>true</checkstyle.skip>
    <java.version>1.7</java.version>
    <slf4j.version>1.7.21</slf4j.version>
    <logback.version>1.1.7</logback.version>
    <junit.version>4.8.2</junit.version>
    <testng.version>6.9.10</testng.version>
    <commons.lang3.version>3.4</commons.lang3.version>
    <spring.version>4.0.9.RELEASE</spring.version>
    <mybatis.version>3.4.0</mybatis.version>
    <jackson.version>2.7.4</jackson.version>
    <dependency.locations.enabled>false</dependency.locations.enabled>
    <antrun.echos.properties>true</antrun.echos.properties>
    <scm.url.base>...</scm.url.base>
  </properties>

  <issueManagement>
    <system>Redmine</system>
    <url>.../issues</url>
  </issueManagement>
  <scm>
    <connection>scm:svn:http:...</connection>
    <url>http:...</url>
  </scm>
  <distributionManagement>
    <repository>
      <id>...</id>
      <name>...</name>
      <url>...</url>
    </repository>
    <snapshotRepository>
      <id>...</id>
      <name>...</name>
      <url>...</url>
    </snapshotRepository>
    <site>
      <id>...</id>
      <url>sftp://.../${project.version}</url>
    </site>
  </distributionManagement>

  <repositories>
    <repository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>thirdstag-releases</id>
      <url>...</url>
    </repository>
    <repository>
      <releases>
        <enabled>false</enabled>
      </releases>
      <snapshots>
        <enabled>true</enabled>
      </snapshots>
      <id>thirdstage-snapshots</id>
      <url>...</url>
    </repository>
    <repository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>central2</id>
      <url>http://repo2.maven.org/maven2/</url>
    </repository>
    <repository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>maven2-repository.dev.java.net</id>
      <name>Java.net Repository for Maven</name>
      <url>http://download.java.net/maven/2/</url>
    </repository>
    <repository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>thirdparty</id>
      <name>3rd-party Repository</name>
      <url>http://repo.expertvill.net/nexus/content/repositories/thirdparty</url>
    </repository>
    <repository>
      <id>evolvis-3rdparty</id>
      <url>http://maven-repo.evolvis.org/3rdparty</url>
    </repository>
  </repositories>
  <pluginRepositories>
    <pluginRepository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>central2</id>
      <url>http://repo2.maven.org/maven2/</url>
    </pluginRepository>
    <pluginRepository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>spring-beandoc</id>
      <url>http://spring-beandoc.sourceforge.net/repo</url>
    </pluginRepository>
    <pluginRepository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>tmatesoft-releases</id>
      <url>http://maven.tmatesoft.com/content/repositories/releases/</url>
    </pluginRepository>
    <pluginRepository>
      <releases>
        <enabled>false</enabled>
      </releases>
      <id>tmatesoft-snapshots</id>
      <url>http://maven.tmatesoft.com/content/repositories/snapshots/</url>
    </pluginRepository>
    <pluginRepository>
      <id>evolvis-3rdparty</id>
      <url>http://maven-repo.evolvis.org/3rdparty</url>
    </pluginRepository>
    <pluginRepository>
      <snapshots>
        <enabled>false</enabled>
      </snapshots>
      <id>elca-services</id>
      <url>http://el4.elca-services.ch/el4j/maven2repository</url>
    </pluginRepository>
  </pluginRepositories>

  <reporting>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-checkstyle-plugin</artifactId>
        <configuration>
          <configLocation>../thirdstage.framework.parent/src/main/config/checkstyle/checkstyle.xml</configLocation>
          <propertyExpansion>parent.basedir=../thirdstage.framework.parent</propertyExpansion>
          <failsOnError>false</failsOnError>
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>findbugs-maven-plugin</artifactId>
        <version>2.3.2</version>
        <configuration>
          <effort>Max</effort>
          <threshold>Low</threshold>
          <onlyAnalyze>thirdstage.framework.*</onlyAnalyze>
          <excludeFilterFile>../thirdstage.framework.parent/src/main/config/findbugs/findbugs-exclude.xml</excludeFilterFile>
          <fork>true</fork>
          <timeout>600000</timeout>
          <maxHeap>512</maxHeap>
        </configuration>
      </plugin>
    </plugins>
  </reporting>

  <build>
    <plugins>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-enforcer-plugin</artifactId>
        <executions>
          <execution>
            <id>enforce-requirements</id>
            <goals>
              <goal>enforce</goal>
            </goals>
            <configuration>
              <!-- For more standard rules, refer http://maven.apache.org/enforcer/enforcer-rules/ -->
              <rules>
                <requireJavaVersion>
                  <version>${java.version}</version>
                </requireJavaVersion>
                <requireMavenVersion>
                  <version>3.0</version>
                </requireMavenVersion>
                <requireEnvironmentVariable>
                  <variableName>OPENCV_DIR</variableName>
                  <variableName>MINGW_HOME</variableName>
                </requireEnvironmentVariable>
              </rules>
            </configuration>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-javadoc-plugin</artifactId>
        <executions>
          <execution>
            <id>attach-javadocs</id>
            <goals>
              <goal>jar</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-jar-plugin</artifactId>
        <executions>
          <execution>
            <goals>
              <goal>test-jar</goal>
            </goals>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-assembly-plugin</artifactId>
        <!-- Unveil the commented block when jar-with-dependencies is necessary
        <executions>
          <execution>
            <id>jar-with-deps</id>
            <phase>package</phase>
            <goals>
              <goal>single</goal>
            </goals>
            <configuration>
              <descriptorRefs>
                <descriptorRef>jar-with-dependencies</descriptorRef>
              </descriptorRefs>
            </configuration>
          </execution>
        </executions>
        -->
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-antrun-plugin</artifactId>
        <executions>
          <execution>
            <id>validate-properties</id>
            <phase>validate</phase>
            <goals>
              <goal>run</goal>
            </goals>
            <configuration>
              <failOnError>true</failOnError>
              <target name="validate-properties">
                <taskdef name="if" classname="ise.antelope.tasks.IfTask" classpathref="maven.plugin.classpath" />

                <if name="antrun.echos.properties" value="true">
                  <echo>Head revision of the project "${project.artifactId}" is
                    ${project.svn.revision}.</echo>
                  <echo>project.artifactId : ${project.artifactId}</echo>
                  <echo>project.basedir : ${project.basedir}</echo>
                  <echo>project.resources.0.directory : ${project.resources.0.directory}</echo>
                  <echo>project.resources[0].directory : ${project.resources[0].directory}</echo>
                  <echo>project.parent : ${project.parent}</echo>
                  <echo>project.parent.groupId : ${project.parent.groupId}</echo>
                  <echo>project.parent.artifactId : ${project.parent.artifactId}</echo>
                  <echo>project.parent.relativePath :
                    ${project.parent.relativePath}</echo>

                  <pathconvert pathsep="${line.separator}" property="classpath.maven.plugin"
                    refid="maven.plugin.classpath" />

                  <pathconvert pathsep="${line.separator}" property="classpath.maven.compile"
                    refid="maven.compile.classpath" />
                  <echo />
                  <echo>Compile-time classpath for Maven :</echo>
                  <echo>${classpath.maven.compile}</echo>

                  <pathconvert pathsep="${line.separator}" property="classpath.maven.runtime"
                    refid="maven.runtime.classpath" />
                  <echo />
                  <echo>Run-time classpath for Maven :</echo>
                  <echo>${classpath.maven.runtime}</echo>

                  <echo>Check Korean output : 한글이 정상적으로 보이나요?</echo>

                  <!-- echo all properties passed to Ant and environment variables prefixed with 'env' -->
                  <echo>All properties given to Ant are : </echo>
                  <property environment="env" />
                  <echoproperties />
                </if>
              </target>
            </configuration>
          </execution>
        </executions>
        <dependencies>
          <dependency>
            <groupId>ise.antelope</groupId>
            <artifactId>ant-antelope-tasks</artifactId>
            <version>3.5.0</version>
          </dependency>
        </dependencies>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-release-plugin</artifactId>
        <configuration>
          <!--
          @fixme javaHome doesn't seem to work as is explained.
          You should specify JAVA_HOME env. variable explicitly in your run configuration when running from
          Eclipse.
           -->
          <!--  <javaHome>c:\lang\jdk1.5</javaHome> -->

          <!--  <mavenHome>C:\tools\maven-3.0.3</mavenHome> -->
          <tagBase>${scm.url.base}/main/tags</tagBase>
          <tagNameFormat>@{project.version}</tagNameFormat>
          <username>OhSangMoon</username>
          <autoVersionSubmodules>true</autoVersionSubmodules>
          <pomFileName>pom.xml</pomFileName>
          <arguments>-f pom.xml</arguments>

          <!--  not supported by release plugin oer 2.2.2
          <generateReleasePoms>true</generateReleasePoms>
          -->
        </configuration>
      </plugin>
      <plugin>
        <groupId>org.apache.maven.plugins</groupId>
        <artifactId>maven-eclipse-plugin</artifactId>
      </plugin>
      <plugin>
        <groupId>org.codehaus.mojo</groupId>
        <artifactId>aspectj-maven-plugin</artifactId>
        <executions>
          <execution>
            <goals>
              <goal>compile</goal>
              <goal>test-compile</goal>
            </goals>
            <configuration>
              <Xlint>warning</Xlint>
            </configuration>
          </execution>
        </executions>
      </plugin>
      <plugin>
        <groupId>org.basepom.maven</groupId>
        <artifactId>duplicate-finder-maven-plugin</artifactId>
        <executions>
          <execution>
            <id>find-duplicate-classes</id>
            <phase>prepare-package</phase>
            <goals><goal>check</goal></goals>
          </execution>
        </executions>
        <configuration>
          <!-- For more, refer https://github.com/basepom/duplicate-finder-maven-plugin/wiki -->
          <skip>false</skip>
          <checkCompileClasspath>false</checkCompileClasspath>
          <checkRuntimeClasspath>true</checkRuntimeClasspath>
          <checkTestClasspath>false</checkTestClasspath>
          <ignoredResourcePatterns>
            <ignoredResourcePattern>about.html</ignoredResourcePattern>
          </ignoredResourcePatterns>
          <ignoredDependencies>
            <dependency>
              <groupId>org.slf4j</groupId>
              <artifactId>jcl-over-slf4j</artifactId>
            </dependency>
          </ignoredDependencies>
        </configuration>
      </plugin>
    </plugins>
    <pluginManagement>
      <plugins>
        <!-- core -->
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-compiler-plugin</artifactId>
          <version>2.3.2</version>
          <inherited>true</inherited>
          <configuration>
            <source>${java.version}</source>
            <target>${java.version}</target>
          </configuration>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-site-plugin</artifactId>
          <version>3.0</version>
          <dependencies>
            <dependency>
              <groupId>org.apache.maven.wagon</groupId>
              <artifactId>wagon-ssh</artifactId>
              <version>2.0</version>
            </dependency>
          </dependencies>
        </plugin>
        <!-- packaging -->
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-jar-plugin</artifactId>
          <version>2.3.2</version>
          <configuration>
            <!-- For more on Maven archiver, refer http://maven.apache.org/shared/maven-archiver/index.html -->
            <archive>
              <addMavenDescriptor>false</addMavenDescriptor>
              <forced>true</forced>
              <index>true</index>
              <manifest>
                <addClasspath>false</addClasspath>
                <addDefaultImplementationEntries>true</addDefaultImplementationEntries>
                <addDefaultSpecificationEntries>true</addDefaultSpecificationEntries>
                <addExtensions>false</addExtensions>
                <classpathLayoutType>simple</classpathLayoutType>
              </manifest>
              <manifestEntries>
                <Source-Revision>${project.svn.revision}</Source-Revision>
              </manifestEntries>
            </archive>
          </configuration>
        </plugin>

        <!-- reporting -->
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-project-info-reports-plugin</artifactId>
          <version>2.4</version>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-javadoc-plugin</artifactId>
          <version>2.8</version>
          <configuration>
            <additionalJOptions>
              <additionalJOption>-Xms128m</additionalJOption>
            </additionalJOptions>
            <docencoding>${project.reporting.outputEncoding}</docencoding>
            <encoding>${project.build.sourceEncoding}</encoding>
            <doctitle>${project.name} ${project.version} API</doctitle>
            <windowtitle>${project.name} ${project.version} API</windowtitle>
            <links>
              <link>http://docs.oracle.com/javase/7/docs/api/</link>
              <link>http://docs.oracle.com/javaee/6/api/</link>
              <link>http://jsr-305.googlecode.com/svn/trunk/javadoc/</link>
              <link>http://docs.jboss.org/hibernate/beanvalidation/spec/1.1/api/</link>
              <link>http://docs.jboss.org/hibernate/validator/5.2/api/</link>
              <link>http://commons.apache.org/proper/commons-lang/javadocs/api-3.3.2/</link>
              <link>http://commons.apache.org/proper/commons-collections/javadocs/api-release/</link>
              <link>http://docs.spring.io/spring/docs/4.0.x/javadoc-api/</link>
            </links>
            <show>projected</show>
            <splitindex>true</splitindex>
            <doclet>org.umlgraph.doclet.UmlGraphDoc</doclet>
            <docletArtifact>
              <groupId>org.umlgraph</groupId>
              <artifactId>umlgraph</artifactId>
              <version>5.6.6</version>
            </docletArtifact>
            <!-- For more on UMLGraph's option, refer http://www.umlgraph.org/doc/indexw.html -->
            <additionalparam>-inferrel</additionalparam>
            <additionalparam>-hide (java|javax).*</additionalparam>
            <additionalparam>-collpackages java.util.*</additionalparam>
            <useStandardDocletOptions>true</useStandardDocletOptions>
          </configuration>
        </plugin>
        <plugin>
          <groupId>org.codehaus.mojo</groupId>
          <artifactId>maven-springbeandoc-plugin</artifactId>
          <version>1.0.8-SNAPSHOT</version>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-checkstyle-plugin</artifactId>
          <version>2.7</version>
        </plugin>
        <plugin>
          <groupId>org.basepom.maven</groupId>
          <artifactId>duplicate-finder-maven-plugin</artifactId>
          <version>1.2.1</version>
        </plugin>
        <!-- tools supporting -->
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-enforcer-plugin</artifactId>
          <version>1.4</version>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-assembly-plugin</artifactId>
          <version>2.2.1</version>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-dependency-plugin</artifactId>
          <version>2.4</version>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-release-plugin</artifactId>
          <version>2.2.2</version>
          <configuration>
            <providerImplementations>
              <svn>javasvn</svn>
            </providerImplementations>
          </configuration>
          <dependencies>
            <dependency>
              <groupId>com.google.code.maven-scm-provider-svnjava</groupId>
              <artifactId>maven-scm-provider-svnjava</artifactId>
              <version>1.15</version>
            </dependency>
            <dependency>
              <groupId>org.tmatesoft.svnkit</groupId>
              <artifactId>svnkit</artifactId>
              <version>1.7.0-SNAPSHOT</version>
            </dependency>
          </dependencies>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-scm-plugin</artifactId>
          <version>1.6</version>
          <configuration>
            <providerImplementations>
              <svn>javasvn</svn>
            </providerImplementations>
          </configuration>
          <dependencies>
            <dependency>
              <groupId>com.google.code.maven-scm-provider-svnjava</groupId>
              <artifactId>maven-scm-provider-svnjava</artifactId>
              <version>1.15</version>
            </dependency>
            <dependency>
              <groupId>org.tmatesoft.svnkit</groupId>
              <artifactId>svnkit</artifactId>
              <version>1.7.0-SNAPSHOT</version>
            </dependency>
          </dependencies>
        </plugin>
        <plugin>
          <!-- http://maven.apache.org/plugins/maven-eclipse-plugin/eclipse-mojo.html -->
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-eclipse-plugin</artifactId>
          <version>2.9</version>
          <configuration>
            <!-- the next two item doesn't work on m2e. m2e has its own confgiruation in Eclipse preferences -->
            <downloadJavadocs>true</downloadJavadocs>
            <downloadSources>true</downloadSources>
            <forceRecheck>false</forceRecheck>
          </configuration>
        </plugin>
        <plugin>
          <groupId>org.apache.maven.plugins</groupId>
          <artifactId>maven-antrun-plugin</artifactId>
          <version>1.7</version>
          <dependencies>
            <dependency>
              <groupId>org.apache.ant</groupId>
              <artifactId>ant</artifactId>
              <version>1.8.2</version>
            </dependency>
          </dependencies>
        </plugin>
        <plugin>
          <groupId>org.codehaus.mojo</groupId>
          <artifactId>aspectj-maven-plugin</artifactId>
          <version>1.8</version>
          <configuration>
            <aspectLibraries>
              <aspectLibrary>
                <groupId>org.springframework</groupId>
                <artifactId>spring-aspects</artifactId>
              </aspectLibrary>
            </aspectLibraries>
            <source>${java.version}</source>
            <target>${java.version}</target>
            <complianceLevel>${java.version}</complianceLevel>
          </configuration>
        </plugin>
        <plugin>
          <groupId>org.codehaus.mojo</groupId>
          <artifactId>properties-maven-plugin</artifactId>
          <version>1.0-alpha-2</version>
        </plugin>
        <plugin>
          <groupId>com.google.code.maven-svn-revision-number-plugin</groupId>
          <artifactId>maven-svn-revision-number-plugin</artifactId>
          <version>1.7</version>
          <executions>
            <execution>
              <phase>validate</phase>
              <goals>
                <goal>revision</goal>
              </goals>
              <configuration>
                <entries>
                  <entry>
                    <path>${project.basedir}</path>
                    <prefix>project.svn</prefix>
                    <depth>infinity</depth>
                    <reportUnversioned>true</reportUnversioned>
                    <reportIgnored>false</reportIgnored>
                    <reportOutOfDate>false</reportOutOfDate>
                  </entry>
                </entries>
              </configuration>
            </execution>
          </executions>
          <dependencies>
            <dependency>
              <groupId>org.tmatesoft.svnkit</groupId>
              <artifactId>svnkit</artifactId>
              <version>1.7.0-SNAPSHOT</version>
            </dependency>
          </dependencies>
        </plugin>
        <plugin>
          <groupId>org.eclipse.m2e</groupId>
          <artifactId>lifecycle-mapping</artifactId>
          <version>1.0.0</version>
          <configuration>
            <lifecycleMappingMetadata>
              <pluginExecutions>
                <pluginExecution>
                  <pluginExecutionFilter>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-dependency-plugin</artifactId>
                    <versionRange>[1.0.0,)</versionRange>
                    <goals>
                      <goal>copy-dependencies</goal>
                    </goals>
                  </pluginExecutionFilter>
                  <action>
                    <ignore />
                  </action>
                </pluginExecution>
                <pluginExecution>
                  <pluginExecutionFilter>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-antrun-plugin</artifactId>
                    <versionRange>[1.0.0,)</versionRange>
                    <goals>
                      <goal>run</goal>
                    </goals>
                  </pluginExecutionFilter>
                  <action>
                    <ignore />
                  </action>
                </pluginExecution>
                <pluginExecution>
                  <pluginExecutionFilter>
                    <groupId>org.codehaus.mojo</groupId>
                    <artifactId>aspectj-maven-plugin</artifactId>
                    <versionRange>[1.0.0,)</versionRange>
                    <goals>
                      <goal>compile</goal>
                      <goal>test-compile</goal>
                    </goals>
                  </pluginExecutionFilter>
                  <action>
                    <ignore />
                  </action>
                </pluginExecution>
                <pluginExecution>
                  <pluginExecutionFilter>
                    <groupId>org.codehaus.mojo</groupId>
                    <artifactId>build-helper-maven-plugin</artifactId>
                    <versionRange>[1.0.0,)</versionRange>
                    <goals>
                      <goal>parse-version</goal>
                    </goals>
                  </pluginExecutionFilter>
                  <action>
                    <ignore />
                  </action>
                </pluginExecution>
                <pluginExecution>
                  <pluginExecutionFilter>
                    <groupId>net.alchim31.maven</groupId>
                    <artifactId>scala-maven-plugin</artifactId>
                    <versionRange>[1.0.0,)</versionRange>
                    <goals>
                      <goal>add-source</goal>
                      <goal>compile</goal>
                      <goal>testCompile</goal>
                    </goals>
                  </pluginExecutionFilter>
                  <action>
                    <ignore/>
                  </action>
                </pluginExecution>
              </pluginExecutions>
            </lifecycleMappingMetadata>
          </configuration>
        </plugin>
      </plugins>
    </pluginManagement>
  </build>

  <dependencies>
    <dependency>
      <groupId>com.google.code.findbugs</groupId>
      <artifactId>jsr305</artifactId>
    </dependency>
    <dependency>
      <groupId>javax.inject</groupId>
      <artifactId>javax.inject</artifactId>
    </dependency>
    <dependency>
      <groupId>javax.validation</groupId>
      <artifactId>validation-api</artifactId>
    </dependency>
    <dependency>
      <groupId>org.hibernate</groupId>
      <artifactId>hibernate-validator</artifactId>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>slf4j-api</artifactId>
    </dependency>
    <dependency>
      <groupId>org.slf4j</groupId>
      <artifactId>jcl-over-slf4j</artifactId>
    </dependency>
    <dependency>
      <groupId>ch.qos.logback</groupId>
      <artifactId>logback-classic</artifactId>
    </dependency>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
    </dependency>
    <dependency>
      <groupId>org.testng</groupId>
      <artifactId>testng</artifactId>
    </dependency>
    <dependency>
      <groupId>org.apache.commons</groupId>
      <artifactId>commons-lang3</artifactId>
    </dependency>
  </dependencies>
  <dependencyManagement>
    <dependencies>
      <dependency>
        <!-- JSR 305 annotations which includes also JCIP annotations -->
        <groupId>com.google.code.findbugs</groupId>
        <artifactId>jsr305</artifactId>
        <version>2.0.3</version>
      </dependency>
      <dependency>
        <!-- JSR 330 annotations -->
        <groupId>javax.inject</groupId>
        <artifactId>javax.inject</artifactId>
        <version>1</version>
      </dependency>
      <dependency>
        <!-- JSR 349 annotations -->
        <groupId>javax.validation</groupId>
        <artifactId>validation-api</artifactId>
        <version>1.1.0.Final</version>
      </dependency>
      <dependency>
        <!-- Hibernate Validator -->
        <groupId>org.hibernate</groupId>
        <artifactId>hibernate-validator</artifactId>
        <version>5.2.4.Final</version>
      </dependency>
      <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>slf4j-api</artifactId>
        <version>${slf4j.version}</version>
      </dependency>
      <dependency>
        <groupId>org.slf4j</groupId>
        <artifactId>jcl-over-slf4j</artifactId>
        <version>${slf4j.version}</version>
      </dependency>
      <dependency>
        <groupId>ch.qos.logback</groupId>
        <artifactId>logback-classic</artifactId>
        <version>${logback.version}</version>
      </dependency>
      <dependency>
        <groupId>org.codehaus.janino</groupId>
        <artifactId>janino</artifactId>
        <version>2.7.8</version>
      </dependency>
      <dependency>
        <groupId>junit</groupId>
        <artifactId>junit</artifactId>
        <version>${junit.version}</version>
        <scope>test</scope>
      </dependency>
      <dependency>
        <groupId>org.testng</groupId>
        <artifactId>testng</artifactId>
        <version>${testng.version}</version>
        <scope>test</scope>
      </dependency>
      <dependency>
        <groupId>org.mockito</groupId>
        <artifactId>mockito-core</artifactId>
        <version>${mockito.version}</version>
        <scope>test</scope>
      </dependency>
      <dependency>
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-lang3</artifactId>
        <version>${commons.lang3.version}</version>
      </dependency>
      <dependency>
        <groupId>commons-io</groupId>
        <artifactId>commons-io</artifactId>
        <version>2.4</version>
      </dependency>
      <dependency>
        <groupId>commons-codec</groupId>
        <artifactId>commons-codec</artifactId>
        <version>1.10</version>
        <scope>test</scope>
      </dependency>
      <dependency>
         <!--For Java 6, Commons DBCP 1.4 MUST be used. -->
        <groupId>commons-dbcp</groupId>
        <artifactId>commons-dbcp</artifactId>
        <version>1.4</version>
      </dependency>
      <dependency>
         <!-- For Java 7, Commons DBCP 2.x MUST be used. -->
         <!-- DBCP 2.x and DBCP 1.4 have difference namespaces. -->
        <groupId>org.apache.commons</groupId>
        <artifactId>commons-dbcp2</artifactId>
        <version>2.1</version>
        <exclusions>
          <exclusion>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
          </exclusion>
        </exclusions>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-core</artifactId>
        <version>${spring.version}</version>
        <exclusions>
          <exclusion>
            <groupId>commons-logging</groupId>
            <artifactId>commons-logging</artifactId>
          </exclusion>
        </exclusions>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-beans</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-context</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-expression</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-aspects</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-jdbc</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-tx</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-web</artifactId>
        <version>${spring.version}</version>
      </dependency>
      <dependency>
        <groupId>org.springframework</groupId>
        <artifactId>spring-test</artifactId>
        <version>${spring.version}</version>
        <scope>test</scope>
      </dependency>
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis</artifactId>
        <version>${mybatis.version}</version>
      </dependency>
      <dependency>
        <groupId>org.mybatis</groupId>
        <artifactId>mybatis-spring</artifactId>
        <version>1.2.2</version>
      </dependency>
      <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-databind</artifactId>
        <version>${jackson.version}</version>
      </dependency>
      <dependency>
        <groupId>com.fasterxml.jackson.core</groupId>
        <artifactId>jackson-annotations</artifactId>
        <version>${jackson.version}</version>
      </dependency>
      <dependency>
        <groupId>com.fasterxml.jackson.module</groupId>
        <artifactId>jackson-module-jaxb-annotations</artifactId>
        <version>${jackson.version}</version>
      </dependency>
    </dependencies>
  </dependencyManagement>
</project>
```

### Using fundamental annotations not included in Java SE

-   JCIP(Java Concurrency in Practice) annotations
    -   `GuardedBy`, `Immutable`, `NotThreadSafe`, `ThreadSafe`
-   JSR 305(Annotations for Software Defect Detection) annotations
    -   `javax.annotation`, `javax.annotation.concurrent`, `javax.annotation.meta` packages
    -   includes JCIP annotations in `javax.annotation.concurrent` package
    -   [JSR305 API](http://jsr-305.googlecode.com/svn/trunk/javadoc/index.html)
-   JSR 330(Dependency Injection for Java) annotations
    -   `javax.inject` package
    -   [javax.inject API](http://docs.oracle.com/javaee/6/api/index.html?javax/inject/package-summary.html)
-   JSR 349(Bean Validation 1.1) annotations
    -   `javax.validation`, `javax.validation.bootstrap`, `javax.validation.constraints` ... packages
    -   [Bean Validation API 1.1.0.Final](http://docs.jboss.org/hibernate/beanvalidation/spec/1.1/api/)
-   Hibernate Validator
    -   [Annotations in Hibernate Validator 5.2](http://docs.jboss.org/hibernate/validator/5.2/api/index.html?org/hibernate/validator/constraints/package-summary.html)

``` xml
  ...
  <dependency>
    <!-- JSR 305 annotations which includes also JCIP annotations -->
    <groupId>com.google.code.findbugs</groupId>
    <artifactId>jsr305</artifactId>
    <version>3.0.1</version>
  </dependency>
  <dependency>
    <!-- JSR 330 annotations -->
    <groupId>javax.inject</groupId>
    <artifactId>javax.inject</artifactId>
    <version>1</version>
  </dependency>
  <dependency>
    <!-- JSR 349 annotations -->
    <groupId>javax.validation</groupId>
    <artifactId>validation-api</artifactId>
    <version>1.1.0.Final</version>
  </dependency>
  <dependency>
    <!-- Hibernate Validator -->
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>5.2.4.Final</version>
  </dependency>
  ...
```

### Defining dependencies on Eclipse SWT and JFace with Maven 2

Artifacts of Eclipse SWT 3.3 and JFace 3.3 are registered central repository, but their versions have qualifiers such as “v3346”, “v20070530” or “I20070606-0010” which are not aligned with Maven's version comparison definition.([http://docs.codehaus.org/display/MAVEN/Dependency+Mediation+and+Conflict+Resolution\#DependencyMediationandConflictResolution-DependencyVersionRanges Dependency Mediation and Conflict Resolution](http://docs.codehaus.org/display/MAVEN/Dependency+Mediation+and+Conflict+Resolution#DependencyMediationandConflictResolution-DependencyVersionRanges_Dependency_Mediation_and_Conflict_Resolution "wikilink")) Nevertheless, Maven 3 seems to be able to resolve the dependencies including transitive ones. But Maven 2 and Maven's Ant Task which uses Maven 2 internally don't resolve the transitive dependencies correctly and throw exception. So, it you want to define dependencies on SWT and JFace with Maven 2 successfully, some transitive dependencies should be excluded and defined as direct ones like the following.

``` xml
  <dependencies>
    <dependency>
      <groupId>org.eclipse.swt.win32.win32</groupId>
      <artifactId>x86</artifactId>
      <version>3.3.0-v3346</version>
      <type>jar</type>
      <scope>compile</scope>
    </dependency>
    <dependency>
      <groupId>org.eclipse</groupId>
      <artifactId>jface</artifactId>
      <version>3.3.0-I20070606-0010</version>
      <type>jar</type>
      <scope>compile</scope>
      <exclusions>
        <exclusion>
          <groupId>org.eclipse</groupId>
          <artifactId>swt</artifactId>
        </exclusion>
        <exclusion>
          <groupId>org.eclipse.core</groupId>
          <artifactId>commands</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
    <dependency>
      <groupId>org.eclipse.jface</groupId>
      <artifactId>text</artifactId>
      <version>3.3.0-v20070606-0010</version>
      <type>jar</type>
      <scope>compile</scope>
      <exclusions>
        <exclusion>
          <groupId>org.eclipse.core</groupId>
          <artifactId>runtime</artifactId>
        </exclusion>
        <exclusion>
          <groupId>org.eclipse</groupId>
          <artifactId>text</artifactId>
        </exclusion>
        <exclusion>
          <groupId>org.eclipse</groupId>
          <artifactId>swt</artifactId>
        </exclusion>
        <exclusion>
          <groupId>org.eclipse</groupId>
          <artifactId>jface</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
    <dependency>
      <groupId>org.eclipse</groupId>
      <artifactId>text</artifactId>
      <version>3.3.0-v20070606-0010</version>
      <type>jar</type>
      <scope>compile</scope>
      <exclusions>
        <exclusion>
          <groupId>org.eclipse.equinox</groupId>
          <artifactId>common</artifactId>
        </exclusion>
      </exclusions>
    </dependency>

    <!-- redundant defintions of transitive dependencies to work with Maven 2 -->
    <dependency>
      <groupId>org.eclipse</groupId>
      <artifactId>swt</artifactId>
      <version>3.3.0-v3346</version>
    </dependency>
    <dependency>
      <groupId>org.eclipse.core</groupId>
      <artifactId>commands</artifactId>
      <version>3.3.0-I20070605-0010</version>
    </dependency>
    <dependency>
      <groupId>org.eclipse.core</groupId>
      <artifactId>runtime</artifactId>
      <version>3.3.100-v20070530</version>
      <exclusions>
        <exclusion>
          <groupId>org.eclipse.equinox</groupId>
          <artifactId>app</artifactId>
        </exclusion>
      </exclusions>
    </dependency>
    <dependency>
      <groupId>org.eclipse.equinox</groupId>
      <artifactId>common</artifactId>
      <version>3.3.0-v20070426</version>
    </dependency>
    <dependency>
      <groupId>org.eclipse.equinox</groupId>
      <artifactId>app</artifactId>
      <version>1.0.0-v20070606</version>
    </dependency>
  </dependencies>
```

### Typical sample of Maven assembly descriptor

The location of deployment descriptors or configuration such as `web.xml`, `log4j.xml` can be adjusted using `file` element of assembly descriptor when they are not under `WEB-INF` or root directory of source directories.

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<assembly
xmlns="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.2"
xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
xsi:schemaLocation="http://maven.apache.org/plugins/maven-assembly-plugin/assembly/1.1.2 http://maven.apache.org/xsd/assembly-1.1.2.xsd">

  <id>view</id>
  <formats>
    <format>jar</format>
  </formats>
  <includeBaseDirectory>false</includeBaseDirectory>
  <fileSets>
    <fileSet>
      <directory>${project.build.outputDirectory}</directory>
      <outputDirectory>/</outputDirectory>
      <useDefaultExcludes>true</useDefaultExcludes>
      <includes>
        <include>ambariplus/view/**/*.class</include>
      </includes>
      <filtered>false</filtered>
      <fileMode>0644</fileMode>
      <directoryMode>0755</directoryMode>
    </fileSet>
  </fileSets>
  <files>
    <file>
      <source>${project.build.outputDirectory}/ambariplus/view/view.xml</source>
      <outputDirectory>/</outputDirectory>
      <destName>view.xml</destName>
      <filtered>false</filtered>
      <lineEnding>unix</lineEnding>
      <fileMode>0644</fileMode>
    </file>
  </files>
</assembly>
```

Ant
---

### Bare Ant build file using Maven-Ant tasks and Antelope

``` xml
<?xml version="1.0" encoding="UTF-8"?>
<project name="project.name" basedir="." default="echo.env" xmlns:artifact="antlib:org.apache.maven.artifact.ant"
  xmlns:antelope="antlib:ise.antelope.tasks" xmlns:contrib="antlib:net.sf.antcontrib">

  <!-- Ant references :
       Manual : http://ant.apache.org/manual/index.html
       Path-like Structures : http://ant.apache.org/manual/using.html#path
       Resource Collections : http://ant.apache.org/manual/Types/resources.html#collection
  -->
  <description>Title or descrption for this build file.</description>

  <property environment="env" />

  <!-- Define Maven-Ant tasks -->
  <!-- For more on Maven-Ant tasks, refer http://maven.apache.org/ant-tasks/ -->
  <typedef resource="org/apache/maven/artifact/ant/antlib.xml"
    uri="antlib:org.apache.maven.artifact.ant"
    classpath="lib/maven-ant-tasks-2.1.3.jar">
  </typedef>

  <artifact:dependencies pathId="ant.tasks.classpath">
    <!-- libraries only necessary for Ant task not application. -->
    <!-- Ant-Contrib tasks. For more, refer http://ant-contrib.sourceforge.net/tasks/ -->
    <dependency groupId="ant-contrib" artifactId="ant-contrib" version="1.0b3" />
    <!-- Antelope tasks. For more, refer http://antelope.stage.tigris.org/nonav/docs/manual/index.html -->
    <dependency groupId="ise.antelope" artifactId="ant-antelope-tasks" version="3.5.0" />
    <!-- SvnAnt tasks. For more, refer http://subclipse.tigris.org/svnant.html -->
    <dependency groupId="org.tigris" artifactId="svnant" version="1.3.1" scope="system"
      systemPath="${basedir}/lib/svnant.jar" />
    <dependency groupId="org.tigris" artifactId="svn-client-adapter" version="0.9.102"
      scope="system" systemPath="${basedir}/lib/svnClientAdapter.jar" />
    <dependency groupId="org.tmatesoft.svnkit" artifactId="svnkit" version="1.8.10" />
    <dependency groupId="org.tmatesoft.svnkit" artifactId="svnkit-javahl16" version="1.8.10" />
    <!-- TestNG tasks. For more, refer http://testng.org/doc/ant.html -->
    <dependency groupId="org.testng" artifactId="testng" version="6.8.21" />
    <dependency groupId="org.mortbay.jetty" artifactId="jetty-runner" version="7.6.9.v20130131" />
    <dependency groupId="org.eclipse.jetty" artifactId="jetty-jmx" version="7.6.9.v20130131" />
    <dependency groupId="org.eclipse.jetty" artifactId="jetty-start" version="7.6.9.v20130131" />
    <dependency groupId="org.umlgraph" artifactId="umlgraph" version="5.6.6" />
    <dependency groupId="com.lunatech.jax-doclets" artifactId="doclets" version="0.10.0" />
    <remoteRepository id="centeral2" url="http://repo2.maven.org/maven2/" />
    <remoteRepository id="maven2-repository.dev.java.net" url="http://download.java.net/maven/2/" />
    <remoteRepository id="tmatesoft-releases"
      url="http://maven.tmatesoft.com/content/repositories/releases/" />
    <remoteRepository id="evolvis-3rdparty" url="http://maven-repo.evolvis.org/3rdparty"
      layout="default" />
  </artifact:dependencies>

  <!-- Define Ant-Contrib tasks -->
  <taskdef resource="net/sf/antcontrib/antlib.xml" uri="antlib:net.sf.antcontrib"
    classpathref="ant.tasks.classpath" />

  <!-- Define Antelope tasks -->
  <taskdef name="stringutil" classname="ise.antelope.tasks.StringUtilTask"
    uri="antlib:ise.antelope.tasks" classpathref="ant.tasks.classpath" />
  <taskdef name="if" classname="ise.antelope.tasks.IfTask"
    uri="antlib:ise.antelope.tasks" classpathref="ant.tasks.classpath" />
  <taskdef name="var" classname="ise.antelope.tasks.Variable"
    uri="antlib:ise.antelope.tasks" classpathref="ant.tasks.classpath" />
  <taskdef name="unset" classname="ise.antelope.tasks.Unset"
    uri="antlib:ise.antelope.tasks" classpathref="ant.tasks.classpath" />

  <!-- Define SvnAnt tasks -->
  <typedef resource="org/tigris/subversion/svnant/svnantlib.xml"
    classpathref="ant.tasks.classpath" />

  <!-- Define TestNG tasks -->
  <taskdef resource="testngtasks" classpathref="ant.tasks.classpath" />

  <artifact:pom id="project.pom" file="${basedir}/pom.xml" />
  <artifact:dependencies pomRefId="project.pom" pathId="project.classpath" />

  <target name="echo.env" depends="query.version.revision">
    <!-- @todo How to sort the list -->
    <pathconvert pathsep="${line.separator}" refid="project.classpath" property="project.classpath.list">
      <mapper type="regexp" from="(.*\\\.m2\\repository\\)?(.*)" to="  \2" />
    </pathconvert>

    <echo>Defined properties : </echo>
    <echoproperties />
    <echo>
    </echo>
    <echo>Classpath : </echo>
    <echo>${project.classpath.list}</echo>
    <echo>Parsed Version : </echo>
    <echo> major : ${ver.major}</echo>
    <echo> minor : ${ver.minor}</echo>
    <echo> incremental : ${ver.incremental}</echo>
    <echo> tail : ${ver.tail}</echo>
    <echo>Max revision under this project : ${revision.max}</echo>
  </target>

  <target name="check.env">
    <fail unless="env.MAVEN_HOME">Environment variable MAVEN_HOME should be specified properly.</fail>
  </target>

  <target name="query.version.revision">
    <svnant:svnSetting id="svn.setting" svnkit="true" javahl="false" />
    <svnant:svn refid="svn.setting">
      <!-- wcversion will set properties of repository.url, revision.max, committed.max and et al.
        For more, refer http://subclipse.tigris.org/svnant/svntask.html#wcversion -->
      <wcversion path="${basedir}" />
    </svnant:svn>
    <contrib:propertyregex property="ver.wo.snapshot" input="${project.pom.version}"
      regexp="([0-9]+(\.[0-9]+)?(\.[0-9]+)?)" select="\1" defaultValue="0.0.0" />

    <contrib:propertyregex property="ver.major" input="${project.pom.version}"
      regexp="([0-9]+)(?:\.([0-9]+))?(?:\.([0-9]+))?(.+)?" select="\1" />
    <contrib:propertyregex property="ver.minor" input="${project.pom.version}"
      regexp="([0-9]+)(?:\.([0-9]+))?(?:\.([0-9]+))?(.+)?" select="\2" defaultValue="0" />
    <contrib:propertyregex property="ver.incremental" input="${project.pom.version}"
      regexp="([0-9]+)(?:\.([0-9]+))?(?:\.([0-9]+))?(.+)?" select="\3" defaultValue="0" />
    <contrib:propertyregex property="ver.tail" input="${project.pom.version}"
      regexp="([0-9]+)(?:\.([0-9]+))?(?:\.([0-9]+))?(.+)?" select="\4" defaultValue="0" />

    <antelope:if name="ver.minor" value="">
      <antelope:unset name="ver.minor" />
      <property name="ver.minor" value="0" />
    </antelope:if>
    <antelope:if name="ver.incremental" value="">
      <antelope:unset name="ver.incremental" />
      <property name="ver.incremental" value="0" />
    </antelope:if>
  </target>
</project>
```

### Start and stop Jetty using Ant

-   **`build.xml`**

``` xml
  <property name="jetty.ver" value="7.6.9.v20130131"/>
  <property name="jetty.port" value="8080"/>
  <property name="jetty.jmxPort" value="3333"/>
  <property name="jetty.stopPort" value="8087"/>
  <property name="jetty.stopKey" value="stopnow"/>

  <artifact:dependencies pathId="jetty.classpath">
    <dependency groupId="org.mortbay.jetty" artifactId="jetty-runner" version="${jetty.ver}"/>
    <dependency groupId="org.eclipse.jetty" artifactId="jetty-start" version="${jetty.ver}"/>
    <dependency groupId="org.eclipse.jetty" artifactId="jetty-jmx" version="${jetty.ver}"/>
    <dependency groupId="org.eclipse.jetty" artifactId="jetty-test-webapp" type="war" version="7.0.0.M2"/>
  </artifact:dependencies>

  <target name="jetty.runner.help"
    description="Print command line help of jetty-runner">
    <java jar="${org.mortbay.jetty:jetty-runner:jar}" fork="true">
      <arg line="--help"/>
    </java>
  </target>

  <target name="jetty.run"
    description="Run Jetty server">
    <java jar="${org.mortbay.jetty:jetty-runner:jar}" fork="true" spawn="false">
      <jvmarg value="-Dcom.sun.management.jmxremote.port=${jetty.jmxPort}"/>
      <jvmarg value="-Dcom.sun.management.jmxremote.ssl=false"/>
      <jvmarg value="-Dcom.sun.management.jmxremote.authenticate=false"/>
      <jvmarg value="-Djetty.host=127.0.0.1"/>
      <jvmarg value="-Djetty.port=${jetty.port}"/>
      <arg value="--log"/>
      <arg value="${basedir}/target/jetty-yyyy_mm_dd.log"/>
      <arg value="--stop-port"/>
      <arg value="${jetty.stopPort}"/>
      <arg value="--stop-key"/>
      <arg value="${jetty.stopKey}"/>
      <arg value="--jar"/>
      <arg value="${org.eclipse.jetty:jetty-jmx:jar}"/>
      <arg value="--config"/>
      <arg value="${basedir}/src/main/webapp/WEB-INF/jetty.xml"/>
      <arg value="${basedir}/src/main/webapp"/>
      <!-- <arg value="${org.eclipse.jetty:jetty-test-webapp:war}"/> -->
    </java>
  </target>

  <target name="jetty.stop"
    description="Stop Jetty server">
    <java jar="${org.eclipse.jetty:jetty-start:jar}" fork="true">
      <jvmarg value="-DSTOP.PORT=${jetty.stopPort}"/>
      <jvmarg value="-DSTOP.KEY=${jetty.stopKey}"/>
      <arg value="--stop"/>
    </java>
  </target>
```

-   **`jetty.xml`**

``` xml
<?xml version="1.0"?>
<!DOCTYPE Configure PUBLIC "-//Jetty//Configure//EN" "http://www.eclipse.org/jetty/configure.dtd">

<Configure id="Server" class="org.eclipse.jetty.server.Server">

  <Set name="ThreadPool">
    <!-- Default queued blocking threadpool -->
    <New class="org.eclipse.jetty.util.thread.QueuedThreadPool">
      <Set name="minThreads">10</Set>
      <Set name="maxThreads">200</Set>
      <Set name="detailedDump">false</Set>
    </New>
  </Set>

  <Call name="addConnector">
    <Arg>
      <New class="org.eclipse.jetty.server.nio.SelectChannelConnector">
        <Set name="host"><SystemProperty name="jetty.host" default="127.0.0.1"/></Set>
        <Set name="port"><SystemProperty name="jetty.port" default="8080" /></Set>
        <Set name="maxIdleTime">300000</Set>
        <Set name="Acceptors">2</Set>
        <Set name="statsOn">false</Set>
        <Set name="confidentialPort">8443</Set>
        <Set name="lowResourcesConnections">20000</Set>
        <Set name="lowResourcesMaxIdleTime">5000</Set>
      </New>
    </Arg>
  </Call>

  <Set name="handler">
    <New id="Handlers" class="org.eclipse.jetty.server.handler.HandlerCollection">
      <Set name="handlers">
        <Array type="org.eclipse.jetty.server.Handler">
          <Item>
            <New id="Contexts" class="org.eclipse.jetty.server.handler.ContextHandlerCollection" />
          </Item>
          <Item>
            <New id="DefaultHandler" class="org.eclipse.jetty.server.handler.DefaultHandler" />
          </Item>
        </Array>
      </Set>
    </New>
  </Set>

  <Set name="stopAtShutdown">true</Set>
  <Set name="sendServerVersion">true</Set>
  <Set name="sendDateHeader">true</Set>
  <Set name="gracefulShutdown">1000</Set>
  <Set name="dumpAfterStart">false</Set>
  <Set name="dumpBeforeStop">false</Set>
</Configure>
```

-   **`web.xml`**
    -   You should set `useFileMappedBuffer` param to `false` to avoid static file locking on Windows.
        -   For more, refer [Deal with Locked Windows Files](http://wiki.eclipse.org/Jetty/Howto/Deal_with_Locked_Windows_Files)

``` xml
<?xml version="1.0" encoding="utf-8"?>
<web-app xmlns="http://java.sun.com/xml/ns/j2ee" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://java.sun.com/xml/ns/j2ee http://java.sun.com/xml/ns/javaee/web-app_2_5.xsd"
  version="2.5">

  <servlet>
    <servlet-name>defaultServlet</servlet-name>
    <servlet-class>org.eclipse.jetty.servlet.DefaultServlet</servlet-class>
    <init-param>
      <param-name>useFileMappedBuffer</param-name>
      <param-value>false</param-value>
    </init-param>
    <load-on-startup>0</load-on-startup>
  </servlet>

  <servlet-mapping>
    <servlet-name>defaultServlet</servlet-name>
    <url-pattern>/</url-pattern>
  </servlet-mapping>

  <welcome-file-list>
    <welcome-file>index.html</welcome-file>
  </welcome-file-list>
  <error-page>
    <error-code>404</error-code>
    <location>/404.html</location>
  </error-page>

</web-app>
```

### Generate Javadoc using Ant and Maven

``` xml
<project name="ambari.server.ext" basedir="."
    xmlns:artifact="antlib:org.apache.maven.artifact.ant">

  <typedef resource="org/apache/maven/artifact/ant/antlib.xml"
     uri="antlib:org.apache.maven.artifact.ant">
    <classpath>
        <pathelement location="lib/maven-ant-tasks-2.1.3.jar"/>
    </classpath>
  </typedef>

  <artifact:dependencies pathId="ambari.server.classpath">
    <pom file="${ambari.server.project.dir}/pom.xml"/>
  </artifact:dependencies>

  <artifact:dependencies pathId="extra.classpath">
    <dependency groupId="org.umlgraph" artifactId="umlgraph" version="5.6.6"/>
  </artifact:dependencies>

  <target name="generateAmbariJavadoc" description="generate Java API documentation for Ambari">
    <javadoc destdir="${ambari.server.project.dir}/target/javadoc"
      maxmemory="256m" access="private" encoding="utf-8" docencoding="utf-8"
      windowtitle="Apache Ambari Java API" doctitle="Apache Ambari Java API"
      version="true" use="true" author="true"   splitindex="true"
      nodeprecated="false" notree="false" noindex="false" nohelp="false" nonavbar="false"
      linksource="yes">
      <sourcepath>
        <pathelement location="${ambari.server.project.dir}/src/main/java"/>
      </sourcepath>
      <classpath refId="ambari.server.classpath"/>
      <doclet name="org.umlgraph.doclet.UmlGraphDoc" path="${org.umlgraph:umlgraph:jar}">
        <param name="-inferrel"/>
        <param name="-hide" value="(java|javax).*"/>
        <param name="-collpackages" value="java.util.*"/>
        <param name="-link" value="http://docs.oracle.com/javase/6/docs/api/"/>
        <param name="-link" value="http://docs.oracle.com/javaee/6/api/"/>
        <param name="-link" value="http://jsr-305.googlecode.com/svn/trunk/javadoc/"/>
        <param name="-link" value="http://docs.jboss.org/hibernate/beanvalidation/spec/1.1/api/"/>
        <param name="-link" value="http://docs.jboss.org/hibernate/validator/5.2/api/"/>
        <param name="-link" value="http://commons.apache.org/proper/commons-lang/javadocs/api-3.3.2/"/>
        <param name="-link" value="http://google-guice.googlecode.com/svn/tags/3.0/javadoc/packages.html"/>
        <param name="-link" value="http://docs.spring.io/spring/docs/4.0.x/javadoc-api/"/>
        <param name="-link" value="https://jersey.java.net/apidocs/1.11/jersey/"/>
      </doclet>
      <link href="http://docs.oracle.com/javase/6/docs/api/"/>
      <link href="http://docs.oracle.com/javaee/6/api/"/>
      <link href="http://jsr-305.googlecode.com/svn/trunk/javadoc/"/>
      <link href="http://docs.jboss.org/hibernate/beanvalidation/spec/1.1/api/"/>
      <link href="http://docs.jboss.org/hibernate/validator/5.2/api/"/>
      <link href="http://commons.apache.org/proper/commons-lang/javadocs/api-3.3.2/"/>
      <link href="http://commons.apache.org/proper/commons-collections/javadocs/api-release/"/>
      <link href="http://google-guice.googlecode.com/svn/tags/3.0/javadoc/packages.html"/>
      <link href="http://docs.spring.io/spring/docs/4.0.x/javadoc-api/"/>
      <link href="https://jersey.java.net/apidocs/1.11/jersey/"/>
    </javadoc>
   </target>
```

It could be better to use deployed artifact instead of the pom.

``` xml
<project name="thirdstage.red5.build" basedir="." default="echo.env"
   xmlns:artifact="antlib:org.apache.maven.artifact.ant">

   <property environment="env"/>

   <typedef resource="org/apache/maven/artifact/ant/antlib.xml"
            uri="antlib:org.apache.maven.artifact.ant">
      <classpath>
         <pathelement location="${basedir}/lib/maven-ant-tasks-2.1.3.jar"/>
      </classpath>
   </typedef>

   <artifact:dependencies pathId="classpath.red5">
      <dependency groupId="org.red5" artifactId="red5-io" version="1.0.5-RELEASE">
         <exclusion groupId="org" artifactId="jaudiotagger"/>
      </dependency>
      <dependency groupId="org.red5" artifactId="red5-server-common" version="1.0.5-RELEASE">
         <exclusion groupId="org" artifactId="jaudiotagger"/>
      </dependency>
      <dependency groupId="org.red5" artifactId="red5-service" version="1.0.5-RELEASE">
         <exclusion groupId="org" artifactId="jaudiotagger"/>
      </dependency>
      <dependency groupId="org.red5" artifactId="red5-server" version="1.0.5-RELEASE">
         <exclusion groupId="org" artifactId="jaudiotagger"/>
      </dependency>
      <dependency groupId="org.red5" artifactId="red5-client" version="1.0.5-RELEASE">
         <exclusion groupId="org" artifactId="jaudiotagger"/>
      </dependency>
      <remoteRepository id="grails" url="https://repo.grails.org/grails/repo/">
         <snapshots checksumPolicy="warn" updatePolicy="never"/>
         <releases checksumPolicy="warn" updatePolicy="never"/>
      </remoteRepository>
   </artifact:dependencies>

   <target name="echo.env">
      <echo>Defined properties : </echo>
      <echoproperties />
   </target>

   <target name="generate.javadoc" description="Generate Javadoc for Red5 projects">
      <javadoc destdir="${basedir}/target/javadoc"
         maxmemory="256m" access="private" encoding="utf-8" docencoding="utf-8"
               windowtitle="Red5 API" doctitle="Red5 API"
               version="true" use="true" author="true" splitindex="true"
               nodeprecated="false" notree="false" noindex="false" nohelp="false" nonavbar="false">
         <sourcepath>
            <pathelement location="${basedir}/../red5-io/src/main/java"/>
            <pathelement location="${basedir}/../red5-server-common/src/main/java"/>
            <pathelement location="${basedir}/../red5-service/src/main/java"/>
            <pathelement location="${basedir}/../red5-server/src/main/java"/>
            <pathelement location="${basedir}/../red5-client/src/main/java"/>
         </sourcepath>
         <classpath refid="classpath.red5"/>
         <link href="http://docs.oracle.com/javase/6/docs/api/"/>
         <link href="http://docs.oracle.com/javaee/6/api/"/>
         <link href="https://mina.apache.org/mina-project/apidocs/"/>
         <link href="http://ehcache.org/apidocs/2.6.9/"/>
      </javadoc>
   </target>
</project>
```

Scala
-----

### Waiting `Enter` key press on the console

``` scala
   ...
    var cnt = 0
    var keyed = false
    do {
      cnt = System.in.available()
      if (cnt >= 1) keyed = true
      else Thread.sleep(500)
    } while (!keyed)

    while (cnt > 0) {
      System.in.read()
      cnt -= cnt
    }
    ...
```

C, C++
------

### Pointer, Address Operator and Indirection(Dereferencing) Operator

The following definitions and explanations are from the ANCI C specicifation

-   **Address and indirection operators**
    -   **Constraints**
        1.  The operand of the unary **&** operator shall be either a function designator, the result of a \[\] or unary **\*** operator, or an lvalue that designates an object that is not a bit-field and is not declared with the register storage-class specifier.
        2.  The operand of the unary **\*** operator shall have pointer type.
    -   **Semantics**
        1.  The unary **&** operator yields the address of its operand. If the operand has type ‘‘type’’, the result has type ‘‘pointer to type’’. If the operand is the result of a unary **\*** operator, neither that operator nor the **&** operator is evaluated and the result is as if both were omitted, except that the constraints on the operators still apply and the result is not an lvalue. Similarly, if the operand is the result of a **\[\]** operator, neither the **&** operator nor the unary **\*** that is implied by the **\[\]** is evaluated and the result is as if the **&** operator were removed and the **\[\]** operator were changed to a **+** operator. Otherwise, the result is a pointer to the object or function designated by its operand.
        2.  The unary **\*** operator denotes indirection. If the operand points to a function, the result is a function designator; if it points to an object, the result is an lvalue designating the object. If the operand has type ‘‘pointer to type’’, the result has type ‘‘type’’. If an invalid value has been assigned to the pointer, the behavior of the unary **\*** operator is undefined.

The following code snippet is from the book 'The C Programming Language'

``` c
int x = 1, y =2, z[10]
int *p;                   /* p is a pointer to int */

p = &x;                   /* p now points to x */
y = *p;                   /* y is now 1 */
*p = 0;                   /* x is now 0 */
p = &z[0];                /* p now points to z[0] */
```

### Developing Visual C++ application using Eclipse CDT

#### Visual C++ 2008 and CDT

##### Visual C++ project : Environment Variables

-   In case of Visual Studio 2008 on Windows 7 64bit, the environment variable are defined at a batch file called from the `C:\Program Files (x86)\Microsoft Visual Studio 9.0\VC\vcvarsall.bat`.

<!-- -->

-   Related files
    -   `C:\Program Files (x86)\Microsoft Visual Studio 9.0\VC\vcvarsall.bat`
    -   `C:\Program Files (x86)\Microsoft Visual Studio 9.0\VC\bin\vcvars32.bat`
    -   `C:\Program Files (x86)\Microsoft Visual Studio 9.0\Common7\Tools\vsvars32.bat`

<!-- -->

-   Defined Variables

| Environment Variable | Value                                                    | Remarks                                                                                                                   |
|----------------------|----------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------|
| VSInstallDir         | C:\\Program Files (x86)\\Microsoft Visual Studio 9.0     |                                                                                                                           |
| VCInstallDir         | C:\\Program Files (x86)\\Microsoft Visual Studio 9.0\\VC |                                                                                                                           |
| WindowsSdkDir        | C:\\Program Files\\Microsoft SDKs\\Windows\\v6.0A\\      | HKEY\_CURRENT\_USER\\Software\\Microsoft\\Microsoft SDKs\\Windows C:\\Program Files (x86)\\Microsoft SDKs\\Windows\\v6.0A |
| FrameworkDir         | C:\\Windows\\Microsoft.NET\\Framework                    |                                                                                                                           |
| FrameworkVersion     | v2.0.50727                                               |                                                                                                                           |
| FrameworkSDKDir      | ?                                                        |                                                                                                                           |
| FxCopDir             | ?                                                        |                                                                                                                           |
| ProgramFiles         | C:\\Program Files                                        | Predefined by Windows 7                                                                                                   |
| SystemRoot           | C:\\Windows                                              | Predefined by Windows 7                                                                                                   |

##### Visual C++ project : Build configuration base

-   Implicit base `path` (`PATH`)

``` dos
$(VCInstallDir)\bin
$(WindowsSdkDir)\bin
$(VSInstallDir)Common7\Tools\bin
$(VSInstallDir)Common7\tools
$(VSInstallDir)Common7\ide
$(ProgramFiles)\HTML Help Workshop
$(FrameworkSDKDir)bin
$(FrameworkDir)$(FrameworkVersion)
$(VSInstallDir)
$(SystemRoot)\SysWow64
$(FxCopDir)
$(PATH)
```

-   Implicit base `include` path (`INCLUDE`)

``` dos
$(VCInstallDir)include
$(VCInstallDir)atlmfc\include
$(WindowsSdkDir)\include
$(FrameworkSDKDir)include
```

-   Implicit `LIBPATH`

``` dos
$(FrameworkDir)$(FrameworkVersion)
$(VCInstallDir)atlmfc\lib
$(VCInstallDir)lib
```

-   Implicit `LIB`

``` dos
$(VCInstallDir)lib
$(VCInstallDir)atlmfc\lib
$(VCInstallDir)atlmfc\lib\i386
$(WindowsSdkDir)\lib
$(FrameworkSDKDir)lib
$(VSInstallDir)
$(VSInstallDir)lib
```

##### CDT Configuration &gt; build variables

| Build Variable   | Value                          | Remarks |
|------------------|--------------------------------|---------|
| VSInstallDir     | ${env\_var:VS\_INSTALL\_DIR}   |         |
| VCInstallDir     | ${env\_var:VC\_INSTALL\_DIR}   |         |
| WindowsSdkDir    | ${env\_var:WINDOWS\_SDK\_DIR}  |         |
| FrameworkDir     | ${env\_var:FRAMEWORK\_DIR}     |         |
| FrameworkVersion | ${env\_var:FRAMEWORK\_VERSION} |         |

##### CDT Configuration &gt; Environment

| Environment Variable | Value                                                                                                                                                                                                                                                                                                       | Remarks |
|----------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| INCLUDE              | ${VCInstallDir}\\include;${VCInstallDir}\\atlmfc\\include;${WindowsSdkDir}\\include;${env\_var:INCLUDE}                                                                                                                                                                                                     |         |
| JAVA\_HOME           | ${env\_var:JAVA\_32\_HOME}                                                                                                                                                                                                                                                                                  |         |
| LIB                  | ${VCInstallDir}\\lib;${VCInstallDir}\\atlmfc\\lib;${VCInstallDir}\\atlmfc\\lib\\i386;${WindowsSdkDir}\\lib;${FrameworkSDKDir}\\lib;${VSInstallDir};${VSInstallDir}\\lib;${env\_var:LIB}                                                                                                                     |         |
| LIBPATH              | ${FrameworkDir}\\${FrameworkVersion};${VCInstallDir}\\atlmfc\\lib;${VCInstallDir}\\lib;${env\_var:LIBPATH}                                                                                                                                                                                                  |         |
| PATH                 | ${VCInstallDir}\\bin;${WindowsSdkDir}\\bin;${VSInstallDir}\\Common7\\Tools\\bin;${VSInstallDir}\\Common7\\Tools;${VSInstallDir}\\Common7\\IDE;${ProgramFiles}\\HTML Help Workshop;${FrameworkDir}\\${FrameworkVersion};${VSInstallDir};${SystemRoot}\\SysWow64;${VCInstallDir}\\VCPackages;${env\_var:PATH} |         |

#### Visual C++ 2010 Express

### Visual C++ Build Configuration Sample : Apache Log4cxx

-   As of Log4cxx 0.10.0

#### Compile

-   **Debug** build

``` dos
/Od /I "..\src\main\include" /I "..\..\apr\include" /I "..\..\apr-util\include" /D "_DEBUG" /D "_USRDLL" /D "DLL_EXPORTS" /D "LOG4CXX" /D "APR_DECLARE_STATIC" /D "APU_DECLARE_STATIC" /D "WIN32" /D "_VC80_UPGRADE=0x0600" /D "_WINDLL" /FD /EHsc /RTC1 /MDd /Fp".\Debug/log4cxx.pch" /Fo".\Debug/" /Fd".\Debug/" /nologo /c /Zi /TP /errorReport:prompt
```

-   **Release** build

``` dos
/O2 /Ob1 /I "..\src\main\include" /I "..\..\apr\include" /I "..\..\apr-util\include" /D "NDEBUG" /D "_USRDLL" /D "DLL_EXPORTS" /D "LOG4CXX" /D "APR_DECLARE_STATIC" /D "APU_DECLARE_STATIC" /D "WIN32" /D "_VC80_UPGRADE=0x0600" /D "_WINDLL" /GF /FD /EHsc /MD /Gy /Fp".\Release/log4cxx.pch" /Fo".\Release/" /Fd".\Release/" /nologo /c /TP /errorReport:prompt
```

#### Link

-   **Debug** build

``` dos
/OUT:".\Debug/log4cxx.dll" /INCREMENTAL:NO /NOLOGO /DLL /MANIFEST /MANIFESTFILE:".\Debug\log4cxx.dll.intermediate.manifest" /MANIFESTUAC:"level='asInvoker' uiAccess='false'" /DEBUG /PDB:".\Debug/log4cxx.pdb" /SUBSYSTEM:CONSOLE /DYNAMICBASE:NO /IMPLIB:".\Debug/log4cxx.lib" /ERRORREPORT:PROMPT ADVAPI32.LIB WS2_32.LIB MSWSOCK.LIB SHELL32.LIB ODBC32.LIB  kernel32.lib user32.lib gdi32.lib winspool.lib comdlg32.lib advapi32.lib shell32.lib ole32.lib oleaut32.lib uuid.lib odbc32.lib odbccp32.lib "..\..\apr-util\libd\aprutil-1.lib" "..\..\apr-util\xml\expat\lib\libd\xml.lib" "..\..\apr\libd\apr-1.lib"
```

-   **Release** build

``` dos
/OUT:".\Release/log4cxx.dll" /INCREMENTAL:NO /NOLOGO /DLL /MANIFEST /MANIFESTFILE:".\Release\log4cxx.dll.intermediate.manifest" /MANIFESTUAC:"level='asInvoker' uiAccess='false'" /PDB:".\Release/log4cxx.pdb" /SUBSYSTEM:CONSOLE /DYNAMICBASE:NO /IMPLIB:".\Release/log4cxx.lib" /ERRORREPORT:PROMPT ADVAPI32.LIB WS2_32.LIB MSWSOCK.LIB SHELL32.LIB ODBC32.LIB  kernel32.lib user32.lib gdi32.lib winspool.lib comdlg32.lib advapi32.lib shell32.lib ole32.lib oleaut32.lib uuid.lib odbc32.lib odbccp32.lib "..\..\apr-util\libd\aprutil-1.lib" "..\..\apr-util\xml\expat\lib\libd\xml.lib" "..\..\apr\libd\apr-1.lib"
```

JavaScript
----------

### Sample to understand scoping and hoisting of JavaScript

The following sample is part of QUnit script to confirm the scope and hoisting of JavaScript explained at <http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html>

``` javascript
test("scope test 1", function(){
  //For explanation, refer http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html

  var foo = 1;

  function bar(){
    var bar = 1;

    if(!foo){ //this block will be EXECUTED. !!!
      var foo = 10; //at interpret time, declare 'var foo' at the 1st line of bar().
      bar = 10;
    };
    return bar;
  };

  function baz(){
    var baz = 1;
    if(foo){ //this block will NOT be executed. !!!
      var foo = 10; //at interpret time, declare 'var foo' at the 1st line of baz().
      baz = 10;
    };
    return baz;
  };

  function quz(){
    var a = 1;
    var quz = 1;
    function a(){ quz = 10; }; //fail to override a, but no error.
    try{
      a(); //will throw error at this line.
    }catch(e){ quz = 100; };
    return quz;
  };

  function kix(){
    b = 1; //global scope
    var kix = 1;
    function b(){ kix = 10; }; //fail to override but no error;
    kix = 100;
    try{
      b(); //will throw error at this line.
    }catch(e){ kix += 1; }
    return kix;
  };

  equal(bar(), 10);
  equal(baz(), 1);
  equal(quz(), 100);
  equal(kix(), 101);
  equal(foo, 1);
});
```

### Creating global object in IIEF

The following sample is a littel modification of `d3.js` which shows :

-   How to encapsulate the global object using IIEF and closuer.
-   Exposing the object into global in case of AMD, RequireJS or non-AMD environment.
-   Prevent the misuse or unintended pollution on `window` or `undefined`.

But the following code doesn't show proper contents in Eclipse Outline view. Need more elaboration.

``` javascript
(function(window, $, undefined){
   "use strict";

   var d3 = { version: "3.4.2" };

   //...

  if (typeof define === "function" && define.amd) {
    define(d3);
  } else if (typeof module === "object" && module.exports) {
    module.exports = d3;
  } else {
    window.d3 = d3;
  }
)(window, window.jQuery);
```

### Basic coordination of chart with D3.js

Use more semantic variables of **`dimensions`**, **`paddings`**, **`area`**, **`domains`** or **`scales`** as objects like the following typical example. Note that some properties of `areas.x`, `area.y`, `domains.x` or `domains.y` are not objects but arrays to make consistency with the syntax of d3.js.

``` javascript
  var dimensions = {width : 800, height : 400};
  var paddings = {top : 20, right : 30, bottom : 40, left : 50};
  var area = {
    x : [paddings.left, dimensions.width - paddings.right],
    y : [dimensions.height - paddings.bottom, paddings.top]  //large value first !
  }
  var domains = {
    x : [new Date(2014,4,1,0,0,0), new Date(2014,4,1,0,1,0)],
    y : [0, 100]
  }
  var r = 3;
  var data = [];

  //create scales
  var scales = {
    x : d3.time.scale().range(area.x).domain(domains.x),
    y : d3.scale.linear().range(area.y).domain(domains.y)
  }

  //create pannel
  d3.select("#chart").insert("svg", "div")
    .attr("width", dimensions.width)
    .attr("height", dimensions.height);
    //.style("background-color", "white");

  var axis = {
    x : d3.svg.axis().scale(scales.x).orient("bottom").ticks(10),
    y : d3.svg.axis().scale(scales.y).orient("left").ticks(10)
  }

  d3.select("#chart svg").append("g")
    .classed("axis x", true)
    .attr("transform", "translate(0, " + area.y[0] + ")")
    .call(axis.x);

  d3.select("#chart svg").append("g")
    .classed("axis y", true)
    .attr("transform", "translate(" + area.x[0] + ", 0)")
    .call(axis.y);
```

HTML and CSS
------------

### Basic templates

#### HTML

``` html5
<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta http-equiv="X-UA-Compatible" content="IE=edge">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>Standard Template</title>
</head>
<body>

</body>
</html>
```

#### CSS

-   References
    -   [The Perfect Paragraph](http://www.smashingmagazine.com/2011/11/29/the-perfect-paragraph/) (November 29th, 2011)
    -   [Design for Developers](http://www.scribd.com/doc/32311867/Design-for-Developers)

``` css
body {
  font-family: "맑은 고딕", Georgia, Verdana, sans-serif;
  font-size: 1.0em;
  line-height:1.5;
  margin-left:2.0em;
  counter-reset: step-h3;
}

pre, code, tt {
  font-family: Consolas,monospace,Courier;
  font-size: 1.0em;
  line-height: 1.3 !important;
}

table { border-collapse: collapse; }
th, td { border: 1px solid #999;padding: 0.2em; }

h1, h2, h3, h4, h5, h6 {
  font-family: "맑은 고딕", Helvetica, "Trebuchet MS", Verdana;
  margin-top:1.0em;
  margin-bottom:1px;
  padding-top:0px;
  padding-bottom:0px;
  -webkit-font-smoothing: antialiased;
}

h1 { font-size:1.8em; }
h2 { font-size:1.5em; }
h3 { font-size:1.4em; }
h4 { font-size:1.3em; }
h5 { font-size:1.2em; }
h6 { font-size:1.15em; }

h3:before {
  content: counter(step-h3) ". ";
  counter-increment: step-h3;
  counter-reset: step-h4;
}
h4:before {
  content: counter(step-h3) ". " counter(step-h4) ". ";
  counter-increment: step-h4;
  counter-reset: step-h5;
}
h5:before {
  content: counter(step-h3) ". " counter(step-h4) ". " counter(step-h5) ". ";
  counter-increment: step-h5;
  counter-reset: step-h6;
}
h6:before {
  content: counter(step-h3) ". " counter(step-h4) ". " counter(step-h5) ". " counter(step-h6) ". ";
  counter-increment: step-h6;
}

div.syntaxhighlighter{
  padding-top:0.5em;
  padding-bottom:0.5em;
  padding-left:0.5em;
  border:thin solid lightgray;
}
div.syntaxhighlighter div.line{
  line-height:1.5 !important;
}
```

XML, XSLT, XPath, XQuery
------------------------

### Useful or intelligent expressions of XQuery

Think over the following expressions. What are they? These are from the book “XSLT Coookbook” by Sal Mangano.

``` xml
(50,45,40,34,32,29,-1)[(index-of((('XXL', 'XL', 'L', 'M', 'S', 'XS')), size), 7)[1]]
```

The following iterates just n times not n<sup>2</sup> times.

``` xml
for $pos in 1 to count($sequence), $item in $sequence[$pos] return $item , $pos
```

### Use positional variable with **`order by`**

Positional variables are calculated before **`oder by`** process. If you want to use positional variables after ordering, you can make additional query with positional variables on ordered query like inline query of SQL.

``` xml
<id-list ordered='true'>
   for $y at $i in (for $x in $doc//@id
      order by string($x)
      return $x)
   return <id count='{$i}'>{string($y)}</id>
</id-list>
```

Regex
-----

### Normalizing start tags of XML

To remove unnecessary white-spaces or line-breaks inside start tags, you can apply two regex pattern nested. In the following JavaScript sample, the tag name is assumed to be more typical than the somewhat complex pattern the XML spec defines.

``` javascript
  function normalizeXmlTags(str){

    var str1, str2;
    var re, re1, re2;
    var result;

    //normalize start tag including attribute values
    //refer http://www.w3.org/TR/REC-xml/#AVNormalize
    //the most common form of xs:name : [A-Za-z_:][-\w:.]+ (http://www.w3.org/TR/xml/#d0e804)
    re1 = /<\s*([A-Za-z_:][-\w:.]+)([^<>]*[^\/<>])?(\/)?>/g;
    re2 = /\s*([A-Za-z_:][-\w:.]+)\s*=\s*(?:"([^"]*)"|'([^']*)')/g;

    str1 = str.replace(re1, function(match, p1, p2, p3, offset, string) {
      p2 = (p2 || "").trim();

      str2 = p2.replace(re2, function(match, p1, p2, p3, offset, string) {
        result = " " + p1 + "=";
        // only one of p2 or p3 always undefined
        if (p2) {
          result += "\"" + p2.trim() + "\"";
        } else if (p3) {
          result += "'" + p3.trim() + "'";
        } else {/*error */ }

        return result;
      });

      return "<" + p1 + str2 + (p3 || "") + ">";
    });

    //normalize the end tag
    re = /<\/\s*([A-Za-z_:][-\w:.]+)\s*>/g;
    return str1.replace(re, "</$1>");
  };
```

SQL
---

### Hierarchical queries

There are two types of hierarchical queries handling tree structure data in adjacency list model (table has recursive relationship using foreign key on parent\_id column). One is **`(START WITH) CONNECT BY`** clause provided by Oracle and the other is **recursive `WITH`** clause(recursive common table expressions). Although `CONNECT BY` clause is much more intuitive, robust and simple, `WITH` clause is ANSI-SQL standard syntax.

#### `CONNECT BY` clause

-   Supported by : Oracle 9i +, DB2 9.7 (the latest version)
-   [Hierarchical Queries of Oracle Database 10g](http://download.oracle.com/docs/cd/B19306_01/server.102/b14200/queries003.htm#i2053935)

``` sql
SELECT ~
FROM ~
WHERE ~
START WITH parent_id IS NULL
CONNECT BY PRIOR id = parent_id
```

#### Recursive `WITH` clause

-   Supported by : DB2, SQL Server 2005+, PostgreSQL 8.4 (the latest version)
-   ANSI-SQL syntax
-   <http://www.ibm.com/developerworks/data/library/techarticle/dm-0510rielau/>
-   <http://msdn.microsoft.com/en-us/library/ms175972.aspx>
-   <http://www.postgresql.org/docs/8.4/interactive/queries-with.html>

#### Examples by database

The structure of the table for the example statements below is defined by the following DDL.

``` sql
create table category(
 id varchar2(10) not null,
 name varchar2(60) not null,
 parent_id varchar2(10),
 seq number(5, 0),
 descn varchar2(2000),
 constraint pk_category primary key (id),
 constraint fk_cateogry_1 foreign key (parent_id) references category(id)
);
```

##### Oracle 9i or higher

The basic query is :

``` sql
select id, name, parent_id, seq, descn,
from category
START WITH parent_id is null
CONNECT BY PRIOR id = parent_id
```

Oracle 9i provides **`LEVEL`** pseudo-column, **`SYS_CONNECT_BY`** function and **`ORDER SIBLINGS BY`** clause which can be used in hierarchical queries.

``` sql
select id, name, parent_id, seq, LEVEL, descn,
       SYS_CONNECT_BY_PATH(id, '//') as id_path,
       SYS_CONNECT_BY_PATH(name, '//') as name_path
from category
START WITH parent_id is null
CONNECT BY PRIOR id = parent_id
ORDER SIBLINGS BY seq, name
```

Oracle 10g provides additional **`CONNECT_BY_ISLEAF`** pseudo-column, **`CONNECT_BY_ISCYCLE`** pseudo-column and **`CONNECT BY NOCYCLE`**.

``` sql
select id, name, parent_id, seq, LEVEL, descn,
       SYS_CONNECT_BY_PATH(id, '//') as id_path,
       SYS_CONNECT_BY_PATH(name, '//') as name_path,
       CONNECT_BY_ISLEAF as is_leaf
from category
START WITH parent_id is null
CONNECT BY NOCYCLE PRIOR id = parent_id and LEVEL >= 5
ORDER SIBLINGS BY seq, name
```

##### Microsoft SQL Server 2005 or higher

The basic hierarchical query is

``` sql
WITH v_category("id", "name", "level", parent_id) as (
   select "id", "name", 1 as "level", parent_id
   from category
   where id is null
   union all
   select a."id", b."level" + 1, a.parent_id
   from category as a inner join v_category as b on a.parent_id = b."id"
)
select "id", "name", "level", parent_id
from v_category
```

#### Readings

-   [Adjacency list model](http://www.sqlsummit.com/AdjacencyList.htm)
-   [Subquery factoring in Oracle 9i](http://www.oracle-developer.net/display.php?id=212)
-   [Recursive Subquery Factoring Examples of Oracle 11.2](http://download.oracle.com/docs/cd/E11882_01/server.112/e10592/statements_10002.htm#BABCDJDB)
    -   Oracle at last added recursive feature to existing with clause.
-   [DB2 9.7: Run Oracle applications on DB2 9.7 for Linux, Unix, and Windows](http://www.ibm.com/developerworks/data/library/techarticle/dm-0907oracleappsondb2/)

#### misc

-   MySQL (up to 6.0, latest version) still doesn't support hierarchical query.

FFmpeg
------

-   [`ffmpeg` Documentation](https://www.ffmpeg.org/ffmpeg.html)
-   [`ffprobe` Documentation](https://www.ffmpeg.org/ffprobe.html)
-   [FFmpeg and H.264 Encoding Guide](https://trac.ffmpeg.org/wiki/Encode/H.264)

### Querying information on a media

``` dos
ffprobe -unit -pretty -print_format json ^
-show_format -show_streams -show_programs -show_chapters ^
-show_program_version  -show_library_versions -show_versions ^
matrix2-superbowltrailer640_dl.mov
```

### Encoding a video using H.264

``` dos
ffmpeg -i matrix2-superbowltrailer640_dl.mov ^
-an -c:v libx264 -r 30 -aspect 16:9 -bf 0 -refs 1 ^
-pix_fmt yuvj420p -preset veryslow -profile:v main -level 4.1 ^
-format avi matrix2-superbowltrailer640_dl.avi
```

``` dos
ffmpeg -i matrix2-superbowltrailer640_dl.mov ^
-an -c:v libx264 -r 30 -aspect 16:9 -bf 0 -refs 1 ^
-pix_fmt yuvj420p -preset veryslow -profile:v main -level 4.1 ^
-format mp4 matrix2-superbowltrailer640_dl.mp4
```

Misc.
-----

### Typical sample of FindBugs' exclude filter

``` xml
<FindBugsFilter>

  <!--
  Details of filter file : "http://findbugs.sourceforge.net/manual/filter.html"
  Details of bug pattern :
    . FindBugs built-in : http://findbugs.sourceforge.net/bugDescriptions.html
    . fb-contrib : http://fb-contrib.sourceforge.net/bugdescriptions.html
  -->

  <!-- GROUP 1. test classes -->
  <Match><Class name="~.*Test[0-9]*"/></Match> <!-- ProcessorTest, ProcessorTest1, ProcessorTest2 -->
  <Match><Class name="~.*Test[0-9]*\$.*"/></Match> <!-- ProcessorTest$Task, ProcessorTest1$Task -->
  <Match><Class name="~.*\.Test[A-Z][a-zA-Z0-9]*"/></Match> <!-- TestProcessor -->

  <!-- GROUP 2. specific to this application or library -->


  <!-- GROUP 3. patterns in LOW priority in most cases -->
  <Match><Bug pattern="BAS_BLOATED_ASSIGNMENT_SCOPE"/></Match>
  <Match><Bug pattern="BX_UNBOXED_AND_COERCED_FOR_TERNARY_OPERATOR"/></Match>
  <Match><Bug pattern="CC_CYCLOMATIC_COMPLEXITY" /></Match>
  <Match><Bug pattern="CD_CIRCULAR_DEPENDENCY" /></Match>
  <Match><Bug pattern="DLC_DUBIOUS_LIST_COLLECTION"/></Match> <!-- fb-contrib -->
  <Match>
      <!-- The following detection find false cases to much as of FindBugs 2.0.2.
      Maybe a bug in the detector -->
      <Bug pattern="DLS_DEAD_LOCAL_STORE"/>
  </Match>
  <Match><Bug pattern="DLS_DEAD_LOCAL_STORE_OF_NULL"/></Match>
  <Match><Bug pattern="DLS_DEAD_LOCAL_STORE_IN_RETURN"/></Match>

  <Match><Bug pattern="DM_CONVERT_CASE"/></Match>

  <Match>
      <!--  Default encoding can be explicitly specified using -Dfile.encoding option
      at JVM's loading time. -->
      <Bug pattern="DM_DEFAULT_ENCODING"/>
  </Match>
  <Match><Bug pattern="DRE_DECLARED_RUNTIME_EXCEPTION"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="EI_EXPOSE_REP" /></Match>
  <Match><Bug pattern="EI_EXPOSE_REP2" /></Match>
  <Match><Bug pattern="EXS_EXCEPTION_SOFTENING_NO_CHECKED" /></Match> <!-- fb-contrib -->
  <Match><Bug pattern="EXS_EXCEPTION_SOFTENING_HAS_CHECKED" /></Match> <!-- fb-contrib -->
  <Match><Bug pattern="EXS_EXCEPTION_SOFTENING_NO_CONSTRAINTS" /></Match> <!-- fb-contrib -->
  <Match><Bug pattern="FCBL_FIELD_COULD_BE_LOCAL" /></Match>
  <Match><Bug pattern="FL_MATH_USING_FLOAT_PRECISION" /></Match>

  <Match>
    <!--
    Performance related bug pattern but in low rank.
    Needs more review before including this pattrn.
    Seems to be included in fb-contrib, but not listed in the official documentation of fb-contrib as of now.
    -->
    <Bug pattern="IMA_INEFFICIENT_MEMBER_ACCESS" />
  </Match>

  <Match><Bug pattern="ITC_INHERITANCE_TYPE_CHECKING" /></Match>
  <Match><Bug pattern="LG_LOST_LOGGER_DUE_TO_WEAK_REFERENCE" /></Match>
  <Match><Bug pattern="LO_LOGGER_LOST_EXCEPTION_STACK_TRACE" /></Match> <!-- fb-contrib -->
  <Match><Bug pattern="LO_STUTTERED_MESSAGE" /></Match> <!-- fb-contrib -->
  <Match><Bug pattern="MDM_INETADDRESS_GETLOCALHOST" /></Match>

  <!-- duplicate with DM_DEFAULT_ENCODING which is one FindBugs built-in pattern -->
  <Match><Bug pattern="MDM_STRING_BYTES_ENCODING" /></Match> <!-- fb-contrib -->

  <Match><Bug pattern="MRC_METHOD_RETURNS_CONSTANT" /></Match>
  <Match><Bug pattern="MS_CANNOT_BE_FINAL" /></Match>
  <Match><Bug pattern="MS_OOI_PKGPROTECT" /></Match>
  <Match><Bug pattern="MS_PKGPROTECT" /></Match> <!-- fb-contrib -->
  <Match><Bug pattern="MS_SHOULD_BE_FINAL"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="NAB_NEEDLESS_BOOLEAN_CONSTANT_CONVERSION"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="NAB_NEEDLESS_BOXING_PARSE"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="NAB_NEEDLESS_BOXING_STRING_CTOR"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="NAB_NEEDLESS_BOX_TO_CAST"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="NAB_NEEDLESS_BOXING_VALUEOF"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="NAB_NEEDLESS_AUTOBOXING_VALUEOF"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="NM_FIELD_NAMING_CONVENTION"/></Match>
  <Match><Bug pattern="NPMC_NON_PRODUCTIVE_METHOD_CALL"/></Match> <!-- fb-contrib -->

  <!-- expreimental. as is described documentation,
    "essentially the same as the OS_OPEN_STREAM and ODR_OPEN_DATABASE_RESOURCE bug patter" -->
  <Match><Bug pattern="OBL_UNSATISFIED_OBLIGATION_EXCEPTION_EDGE"/></Match>

  <Match><Bug pattern="OC_OVERZEALOUS_CASTING"/></Match>
  <Match><Bug pattern="OCP_OVERLY_CONCRETE_PARAMETER"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="PCAIL_POSSIBLE_CONSTANT_ALLOCATION_IN_LOOP"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="PCOA_PARTIALLY_CONSTRUCTED_OBJECT_ACCESS"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="RCN_REDUNDANT_NULLCHECK_OF_NONNULL_VALUE"/></Match>
  <Match><Bug pattern="RCN_REDUNDANT_NULLCHECK_OF_NULL_VALUE"/></Match>
  <Match><Bug pattern="PMB_POSSIBLE_MEMORY_BLOAT"/></Match>
  <Match><Bug pattern="PRMC_POSSIBLY_REDUNDANT_METHOD_CALLS"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="REC_CATCH_EXCEPTION" /></Match>
  <Match><Bug pattern="PZLA_PREFER_ZERO_LENGTH_ARRAYS" /></Match>
  <Match><Bug pattern="SBSC_USE_STRINGBUFFER_CONCATENATION" /></Match>
  <Match><Bug pattern="SE_NO_SERIALVERSIONID" /></Match>
  <Match><Bug pattern="SIC_INNER_SHOULD_BE_STATIC_ANON" /></Match>

  <!-- seems to work incorrectly with 2.0.2 -->
  <Match><Bug pattern="SNG_SUSPICIOUS_NULL_FIELD_GUARD" /></Match>
  <Match><Bug pattern="SPP_TEMPORARY_TRIM"/></Match> <!-- fb-contrib : clarity -->
  <Match><Bug pattern="SPP_SUSPECT_STRING_TEST"/></Match>
  <Match><Bug pattern="SPP_USE_ISEMPTY"/></Match> <!-- fb-contrib : clarity -->
  <Match><Bug pattern="SPP_USE_MATH_CONSTANT"/></Match>
  <Match><Bug pattern="ST_WRITE_TO_STATIC_FROM_INSTANCE_METHOD" /></Match>
  <Match><Bug pattern="SS_SHOULD_BE_STATIC" /></Match>
  <Match><Bug pattern="SUA_SUSPICIOUS_UNINITIALIZED_ARRAY"/></Match>
  <Match><Bug pattern="UNNC_UNNECESSARY_NEW_NULL_CHECK"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="UCPM_USE_CHARACTER_PARAMETERIZED_METHOD"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="UI_INHERITANCE_UNSAFE_GETRESOURCE" /></Match>
  <Match><Bug pattern="URF_UNREAD_FIELD" /></Match>
  <Match><Bug pattern="URF_UNREAD_PUBLIC_OR_PROTECTED_FIELD" /></Match>
  <Match><Bug pattern="USBR_UNNECESSARY_STORE_BEFORE_RETURN"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="UUF_UNUSED_PUBLIC_OR_PROTECTED_FIELD"/></Match>
  <Match><Bug pattern="UVA_USE_VAR_ARGS"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="WEM_WEAK_EXCEPTION_MESSAGING"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="WOC_WRITE_ONLY_COLLECTION_FIELD"/></Match> <!-- fb-contrib -->
  <Match><Bug pattern="WOC_WRITE_ONLY_COLLECTION_LOCAL"/></Match> <!-- fb-contrib -->
</FindBugsFilter>
```

### Typical sample of PMD ruleset configuration

-   [How to make a new rule set](https://pmd.github.io/pmd-5.4.1/customizing/howtomakearuleset.html)

``` javascript
<?xml version="1.0"?>
<ruleset name="Custom ruleset" xmlns="http://pmd.sourceforge.net/ruleset/2.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="http://pmd.sourceforge.net/ruleset/2.0.0 http://pmd.sourceforge.net/ruleset_2_0_0.xsd">
  <description>
  Ruleset for PMD
  </description>

  <rule ref="rulesets/java/basic.xml">
  </rule>

  <rule ref="rulesets/java/braces.xml">
  </rule>

  <rule ref="rulesets/java/clone.xml">
  </rule>

  <rule ref="rulesets/java/codesize.xml">
  </rule>

  <rule ref="rulesets/java/comments.xml">
    <exclude name="CommentSize" />
  </rule>

  <rule ref="rulesets/java/controversial.xml">
  </rule>

  <rule ref="rulesets/java/coupling.xml">
  </rule>

  <rule name="ControllerClassNamePostfix" class="net.sourceforge.pmd.lang.rule.XPathRule" language="java"
    message="Controller classes under packages ending with '.controller' should have 'Controller' or 'ControllerTest' postfix in their name">
    <description>Controller classes under packages ending with '.controller' should have 'Controller' postfix in their name</description>
    <priority>1</priority>
    <properties>
      <property name="xpath">
        <value>
        <![CDATA[//CompilationUnit[matches(PackageDeclaration/Name/@Image, '.*\.controller')]//ClassOrInterfaceDeclaration[@Public and not(matches(@Image, '.*Controller(Test)?$'))]]]>
        </value>
      </property>
    </properties>
    <example>
    <![CDATA[
      lucy.activity.controller.ActivityTypeController
    ]]>
    </example>
  </rule>

</ruleset>
```

### Typical sample of JSHint configuration

-   [JSHint Options](http://www.jshint.com/docs/options/)

``` javascript
{
  "eqeqeq": true,
  //"freeze": true,
  "noarg": true,
  "smarttabs": true,
  "undef": true,
  "unused": true,
  "-W099": true, // allowed mixed tabs and spaces

  "laxbreak": true,

  "browser": true,
  "devel": true,
  "jquery": true,
  "predef": ["require", "Ember", "Em", "CodeMirror", "vkbeautify"]
}
```

### JSON schema examples

The following examples are from [the home of JSON Schema](http://json-schema.org/).

-   **A simple example**

``` javascript
{
    "$schema": "http://json-schema.org/draft-04/schema#",
    "title": "Product set",
    "type": "array",
    "items": {
        "title": "Product",
        "type": "object",
        "properties": {
            "id": {
                "description": "The unique identifier for a product",
                "type": "number"
            },
            "name": {
                "type": "string"
            },
            "price": {
                "type": "number",
                "minimum": 0,
                "exclusiveMinimum": true
            },
            "tags": {
                "type": "array",
                "items": {
                    "type": "string"
                },
                "minItems": 1,
                "uniqueItems": true
            },
            "dimensions": {
                "type": "object",
                "properties": {
                    "length": {"type": "number"},
                    "width": {"type": "number"},
                    "height": {"type": "number"}
                },
                "required": ["length", "width", "height"]
            },
            "warehouseLocation": {
                "description": "Coordinates of the warehouse with the product",
                "$ref": "http://json-schema.org/geo"
            }
        },
        "required": ["id", "name", "price"]
    }
}
```

-   **An advanced example**

``` javascript
{
    "id": "http://some.site.somewhere/entry-schema#",
    "$schema": "http://json-schema.org/draft-04/schema#",
    "description": "schema for an fstab entry",
    "type": "object",
    "required": [ "storage" ],
    "properties": {
        "storage": {
            "type": "object",
            "oneOf": [
                { "$ref": "#/definitions/diskDevice" },
                { "$ref": "#/definitions/diskUUID" },
                { "$ref": "#/definitions/nfs" },
                { "$ref": "#/definitions/tmpfs" }
            ]
        },
        "fstype": {
            "enum": [ "ext3", "ext4", "btrfs" ]
        },
        "options": {
            "type": "array",
            "minItems": 1,
            "items": { "type": "string" },
            "uniqueItems": true
        },
        "readonly": { "type": "boolean" }
    },
    "definitions": {
        "diskDevice": {
            "properties": {
                "type": { "enum": [ "disk" ] },
                "device": {
                    "type": "string",
                    "pattern": "^/dev/[^/]+(/[^/]+)*$"
                }
            },
            "required": [ "type", "device" ],
            "additionalProperties": false
        },
        "diskUUID": {
            "properties": {
                "type": { "enum": [ "disk" ] },
                "label": {
                    "type": "string",
                    "pattern": "^[a-fA-F0-9]{8}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{4}-[a-fA-F0-9]{12}$"
                }
            },
            "required": [ "type", "label" ],
            "additionalProperties": false
        },
        "nfs": {
            "properties": {
                "type": { "enum": [ "nfs" ] },
                "remotePath": {
                    "type": "string",
                    "pattern": "^(/[^/]+)+$"
                },
                "server": {
                    "type": "string",
                    "oneOf": [
                        { "format": "host-name" },
                        { "format": "ipv4" },
                        { "format": "ipv6" }
                    ]
                }
            },
            "required": [ "type", "server", "remotePath" ],
            "additionalProperties": false
        },
        "tmpfs": {
            "properties": {
                "type": { "enum": [ "tmpfs" ] },
                "sizeInMB": {
                    "type": "integer",
                    "minimum": 16,
                    "maximum": 512
                }
            },
            "required": [ "type", "sizeInMB" ],
            "additionalProperties": false
        }
    }
}
```

#### JSON Core/Validation Meta-Schema

-   Used for schemas written for pure validation.

``` javascript
{
    "id": "http://json-schema.org/draft-04/schema#",
    "$schema": "http://json-schema.org/draft-04/schema#",
    "description": "Core schema meta-schema",
    "definitions": {
        "schemaArray": {
            "type": "array",
            "minItems": 1,
            "items": { "$ref": "#" }
        },
        "positiveInteger": {
            "type": "integer",
            "minimum": 0
        },
        "positiveIntegerDefault0": {
            "allOf": [ { "$ref": "#/definitions/positiveInteger" }, { "default": 0 } ]
        },
        "simpleTypes": {
            "enum": [ "array", "boolean", "integer", "null", "number", "object", "string" ]
        },
        "stringArray": {
            "type": "array",
            "items": { "type": "string" },
            "minItems": 1,
            "uniqueItems": true
        }
    },
    "type": "object",
    "properties": {
        "id": {
            "type": "string",
            "format": "uri"
        },
        "$schema": {
            "type": "string",
            "format": "uri"
        },
        "title": {
            "type": "string"
        },
        "description": {
            "type": "string"
        },
        "default": {},
        "multipleOf": {
            "type": "number",
            "minimum": 0,
            "exclusiveMinimum": true
        },
        "maximum": {
            "type": "number"
        },
        "exclusiveMaximum": {
            "type": "boolean",
            "default": false
        },
        "minimum": {
            "type": "number"
        },
        "exclusiveMinimum": {
            "type": "boolean",
            "default": false
        },
        "maxLength": { "$ref": "#/definitions/positiveInteger" },
        "minLength": { "$ref": "#/definitions/positiveIntegerDefault0" },
        "pattern": {
            "type": "string",
            "format": "regex"
        },
        "additionalItems": {
            "anyOf": [
                { "type": "boolean" },
                { "$ref": "#" }
            ],
            "default": {}
        },
        "items": {
            "anyOf": [
                { "$ref": "#" },
                { "$ref": "#/definitions/schemaArray" }
            ],
            "default": {}
        },
        "maxItems": { "$ref": "#/definitions/positiveInteger" },
        "minItems": { "$ref": "#/definitions/positiveIntegerDefault0" },
        "uniqueItems": {
            "type": "boolean",
            "default": false
        },
        "maxProperties": { "$ref": "#/definitions/positiveInteger" },
        "minProperties": { "$ref": "#/definitions/positiveIntegerDefault0" },
        "required": { "$ref": "#/definitions/stringArray" },
        "additionalProperties": {
            "anyOf": [
                { "type": "boolean" },
                { "$ref": "#" }
            ],
            "default": {}
        },
        "definitions": {
            "type": "object",
            "additionalProperties": { "$ref": "#" },
            "default": {}
        },
        "properties": {
            "type": "object",
            "additionalProperties": { "$ref": "#" },
            "default": {}
        },
        "patternProperties": {
            "type": "object",
            "additionalProperties": { "$ref": "#" },
            "default": {}
        },
        "dependencies": {
            "type": "object",
            "additionalProperties": {
                "anyOf": [
                    { "$ref": "#" },
                    { "$ref": "#/definitions/stringArray" }
                ]
            }
        },
        "enum": {
            "type": "array",
            "minItems": 1,
            "uniqueItems": true
        },
        "type": {
            "anyOf": [
                { "$ref": "#/definitions/simpleTypes" },
                {
                    "type": "array",
                    "items": { "$ref": "#/definitions/simpleTypes" },
                    "minItems": 1,
                    "uniqueItems": true
                }
            ]
        },
        "allOf": { "$ref": "#/definitions/schemaArray" },
        "anyOf": { "$ref": "#/definitions/schemaArray" },
        "oneOf": { "$ref": "#/definitions/schemaArray" },
        "not": { "$ref": "#" }
    },
    "dependencies": {
        "exclusiveMaximum": [ "maximum" ],
        "exclusiveMinimum": [ "minimum" ]
    },
    "default": {}
}
```

### Configuration exclusion list samples by product

#### Git

A typical `.gitignore` file contains

``` bash
bin/
target/
build/
output/
Debug/
Release/
x86/
x64/
obj/
node_modules/
log/
logs/
test-output/
reports/
.apt_generated/
/**/*.log
/**/.cache*
/**/.~*
```

For more, refer

-   [`gitignore` documentation](https://git-scm.com/docs/gitignore)
-   [A collection of useful `.gitignore` templates](https://github.com/github/gitignore)

#### Subversion

A typical [`svn:ignore`](svn:ignore) property contains

``` bash
bin
target
build
output
Debug
Release
x86
x64
obj
node_modules
log
logs
test-output
reports
*.log
```

#### Team Foundation Server

A typical `.tfignore` file contains

``` bash
/bin
/target
/build
/output
/Debug
/Release
/x86
/x64
/obj
/node_modules
/log
/logs
/test-output
/reports
/*.log
```

### Git per-repository settings(`.gitattributes` file) sample

The following `.gitattributes` prohibits the automatic conversion of line ending by Git. This is especially important with Eclipses project when project specific line ending and text encoding are defined.

``` bash
# 2016/01/07 - added C++ sources (*.cpp and *.hpp)

# Set the default behavior, in case people don't have core.autocrlf set.
* -text

# Explicitly declare text files you want to always be normalized and converted
# to native line endings on checkout.
*.c -text
*.cpp -text
*.h -text
*.hpp -text
*.java -text
*.properties -text
*.jsp -text
*.jspf -text
*.tag -text
*.MF -text
*.aj -text
*.ftl -text
*.html -text
*.htm -text
*.xhtml -text
*.css -text
*.js -text
*.json -text
*.xml -text
*.dtd -text
*.xsd -text
*.sch -text
*.xsl -text
*.xslt -text
*.wsdl -text
*.wsdd -text
*.svg -text
*.mediawiki -text
*.textile -text
*.sql -text
*.ddl -text
*.q -text
*.g -text
*.tokens -text
*.script -text
*.cfg -text
.classpath -text
.project -text
.jsdtscope -text
*.prefs -text
*.component -text
*.launch -text
.springBeans -text
.euml2 -text
.umlproject -text
.clay -text
.fbprefs -text
.pmd -text
.lint4jprefs -text
.jshintrc -text

# Declare files that will always have CRLF line endings on checkout.
*.sln text eol=crlf

# Denote all files that are truly binary and should not be modified.
*.a binary
*.o binary
*.so binary
*.lib binary
*.com binary
*.exe binary
*.dll binary
*.zip binary
*.7z binary
*.png binary
*.jpeg binary
*.jpg binary
*.gif binary
*.bmp binary
*.tif binary
*.tiff binary
*.mp3 binary
*.mp4 binary
*.avi binary
*.flv binary
*.ico binary
*.class binary
*.jar binary
*.doc binary
*.docx binary
*.xls binary
*.xlsx binary
*.ppt binary
*.pptx binary
*.pdf binary
```

-   Reference
    -   [GitHub Help &gt; Dealing with line endings](https://help.github.com/articles/dealing-with-line-endings/)

### Typical sample of Wix Toolset source

``` xml
<?xml version='1.0' encoding='windows-1252'?>
<Wix xmlns='http://schemas.microsoft.com/wix/2006/wi'>
   <!--
   WiX source for NEXCORE VAS library installer

   author : Sangmoon Oh
   since : 2015/05/27

   This file contains lots of placeholders following Ant syntax (@TOKEN@) and is
   expected to be filtered by Ant or Maven before processed by WiX compiler

   Required token
     * VAS_VERSION
     * PROJECT_DIR
     * MAIN_JAR_PATH : Full path (including file name and extension) for the main Jar file
     * TEST_JAR_PATH : Full path (including file name and extension) for the test Jar file
     * RUNTIME_CLASSPATH : Classpath from the Jars in runtime scope
     * TEST_CLASSPATH : Classpath from the Jars in test scope
     * DEPENDENT_JARS_ZIP_PATH : Full path for the Zip file containing the Jar files on which this project depends
     * ADAPTER_VA_SKT_PROJECT_DIR
     * ADAPTER_VA_SKT_BUILD_CONFIG : C project build configuration name such as 'Debug', 'Release' or something like that
     * ADAPTER_VA_SKT_ARTIFACT_NAME : File name of DLL without path and extension.
     * ADAPTER_VMS_TRIUMI_PROJECT_DIR
     * ADAPTER_VMS_TRIUMI_BUILD_CONFIG
     * ADAPTER_VMS_TRIUMI_ARTIFACT_NAME
     * OPENCV_BASE
   -->

   <!--
   @TODO Try to directly inject Ant properties and Maven properties exposed to Ant to simplify this docuement.
   @TODO(Done) Add opencv_java249.dll
   @TODO(Done) Add log4cxx.properties and logback.xml
   @TODO Installer create or update environment variables 'NEXCORE_VAS_HOME', 'NEXCORE_VAS_DATA_DIR' and ''NEXCORE_VAS_LOG_DIR'
   @TODO Add sample VF1 file in SECU_ADAPTER_VMS_TRIUMI project, for test.
   @TODO Unzip the zip file containing dependent Jars.
   @TODO Add steps to check if the required vc++ runtime is installed or not using ProductSearch or something like that.
   @TODO Add steps to install vc++ runtime if necessary during installation
   @TODO Separate the OpenCV into another package.
   @TODO Add maven-ant-tasks-2.1.3.jar into test\lib\
   @TODO Add test\resources\com\skcc\nexcore\vas\test-ant.xml
   -->

   <!-- Don't change the Id and Name of Product as possible -->
   <Product Id="4F8D46B3-287B-4894-8387-2AC52A68BAFD"
      UpgradeCode="7AF39E75-2F15-4066-AD7D-D406980683F3"
      Name="NEXCORE Video Analysis Service Library" Version="@VAS_VERSION@" Language="1033"
      Codepage="1252" Manufacturer="SK C&amp;C">
      <Package InstallerVersion="100" Languages="1033" SummaryCodepage="1252"
         InstallScope="perMachine"
         Compressed="yes" Keywords="NEXCORE VAS,NEXCORE"
         Comments="NEXCORE VAS Library for Windows"
         Description="DLLs and their related artifacts for NEXCORE Video Analysis Service" />
      <Media Id="1" Cabinet="nexcore_vas.cab" EmbedCab="yes" />

      <!-- Properties and variables -->
      <Property Id="ApplicationFolderName" Value="NEXCORE VAS" />
      <Property Id="WixAppFolder" Value="WixPerMachineFolder" />
      <WixVariable Id="WixUISupportPerUser" Value="0" />

      <!-- Directory layout -->
      <Directory Id="TARGETDIR" Name="SourceDir">
         <Directory Id="ProgramFilesFolder">
            <Directory Id="APPLICATIONFOLDER">
               <Directory Id="INCLUDE_DIR" Name="include">
                  <!-- @TODO Can be replaced by Component[Id="DIRECTORIES"] element below -->
                  <Component Id="INCLUDE_DIR_CREATE" Guid=""/>
               </Directory>
               <Directory Id="BIN_DIR" Name="bin">
                  <Directory Id="BIN_X86_DIR" Name="x86">
                     <Directory Id="BIN_X86_VC9_DIR" Name="vc9" />
                     <Directory Id="BIN_X86_VC10_DIR" Name="vc10" />
                  </Directory>
               </Directory>
               <Directory Id="LIB_DIR" Name="lib">
                  <Directory Id="LIB_X86_DIR" Name="x86">
                     <Directory Id="LIB_X86_VC9_DIR" Name="vc9" />
                     <Directory Id="LIB_X86_VC10_DIR" Name="vc10" />
                  </Directory>
               </Directory>
               <Directory Id="CONF_DIR" Name="conf"/>
               <Directory Id="DATA_DIR" Name="data">
                  <Directory Id="DATA_VIDEO_DIR" Name="video">
                     <Directory Id="DATA_VIDEO_TRIUMI_DIR" Name="triumi">
                        <Directory Id="DATA_VIDEO_TRIUMI_SAMPLES_DIR" Name="samples">
                           <Directory Id="DATA_VIDEO_TRIUMI_SAMPLES_SET1_DIR" Name="set1"/>
                        </Directory>
                     </Directory>
                  </Directory>
               </Directory>
               <Directory Id="DOC_DIR" Name="doc">
                  <Directory Id="DOC_API_DIR" Name="api">
                     <Directory Id="DOC_API_JAVADOC_DIR" Name="javadoc"/>
                  </Directory>
               </Directory>
               <Directory Id="LOG_DIR" Name="log"/>
               <Directory Id="TMP_DIR" Name="tmp"/>
               <Directory Id="TEST_DIR" Name="test">
                  <Directory Id="TEST_LIB_DIR" Name="lib"/>
                  <Directory Id="TEST_CLASSES_DIR" Name="classes"/>
                  <Directory Id="TEST_OUTPUT_DIR" Name="output"/>
               </Directory>
               <Directory Id="THIRD_PARTY_BASE" Name="3rdparty">
                  <Directory Id="SKT_VC_BASE" Name="sktvc">
                     <!-- Location for libraries from SKT's Video Cloud -->
                     <Directory Id="SKT_VC_BIN_DIR" Name="bin">
                        <Directory Id="SKT_VC_BIN_X86_DIR" Name="x86">
                           <Directory Id="SKT_VC_BIN_X86_VC10_DIR" Name="vc10" />
                        </Directory>
                     </Directory>
                     <Directory Id="SKT_VC_LIB_DIR" Name="lib">
                        <Directory Id="SKT_VC_LIB_X86_DIR" Name="x86">
                           <Directory Id="SKT_VC_LIB_X86_VC10_DIR" Name="vc10" />
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="TRIUMI_BASE" Name="triumi">
                     <Directory Id="TRIUMI_BIN_DIR" Name="bin">
                        <Directory Id="TRIUMI_BIN_X86_DIR" Name="x86">
                           <Directory Id="TRIUMI_BIN_X86_KO_DIR" Name="ko-KR" />
                        </Directory>
                     </Directory>
                     <Directory Id="TRIUMI_LIB_DIR" Name="lib">
                        <Directory Id="TRIUMI_LIB_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="LOG4CXX_BASE" Name="log4cxx-0.10">
                     <Directory Id="LOG4CXX_BIN_DIR" Name="bin">
                        <Directory Id="LOG4CXX_BIN_X86_DIR" Name="x86">
                           <Directory Id="LOG4CXX_BIN_X86_VC9_DIR" Name="vc9" />
                        </Directory>
                     </Directory>
                     <Directory Id="LOG4CXX_LIB_DIR" Name="lib">
                        <Directory Id="LOG4CXX_LIB_X86_DIR" Name="x86">
                           <Directory Id="LOG4CXX_LIB_X86_VC9_DIR" Name="vc9" />
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="OPENCV_BASE" Name="opencv-2.4.9">
                     <Directory Id="OPENCV_BIN_DIR" Name="bin">
                        <Directory Id="OPENCV_BIN_X86_DIR" Name="x86">
                           <Directory Id="OPENCV_BIN_X86_VC10_DIR" Name="vc10" />
                        </Directory>
                     </Directory>
                     <Directory Id="OPENCV_LIB_DIR" Name="lib">
                        <Directory Id="OPENCV_LIB_X86_DIR" Name="x86">
                           <Directory Id="OPENCV_LIB_X86_VC10_DIR" Name="vc10" />
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="EMGU_CV_BASE" Name="emgucv-2.4.2">
                     <Directory Id="EMGU_CV_BIN_DIR" Name="bin">
                        <Directory Id="EMGU_CV_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="OPENCV_SHARP_BASE" Name="opencvsharp">
                     <Directory Id="OPENCV_SHARP_BIN_DIR" Name="bin">
                        <Directory Id="OPENCV_SHARP_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="FFMPEG_BASE" Name="ffmpeg">
                     <Directory Id="FFMPEG_BIN_DIR" Name="bin">
                        <Directory Id="FFMPEG_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="INTEL_JPEG_BASE" Name="ijl">
                     <Directory Id="INTEL_JPEG_BIN_DIR" Name="bin">
                        <Directory Id="INTEL_JPEG_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="GENUINE_CHANNELS_BASE" Name="genuine-channels-2.5">
                     <Directory Id="GENUINE_CHANNELS_BIN_DIR" Name="bin">
                        <Directory Id="GENUINE_CHANNELS_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="SMART_THREAD_POOL_BASE" Name="smart-thread-pool">
                     <Directory Id="SMART_THREAD_POOL_BIN_DIR" Name="bin">
                        <Directory Id="SMART_THREAD_POOL_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="SHARP_ZIP_BASE" Name="sharp-zip-lib-0.84">
                     <Directory Id="SHARP_ZIP_BIN_DIR" Name="bin">
                        <Directory Id="SHARP_ZIP_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="JSON_NET_BASE" Name="json-net-6.0">
                     <Directory Id="JSON_NET_BIN_DIR" Name="bin">
                        <Directory Id="JSON_NET_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
                  <Directory Id="VNC_SHARP_BASE" Name="vncsharp-1.0">
                     <Directory Id="VNC_SHARP_BIN_DIR" Name="bin">
                        <Directory Id="VNC_SHARP_BIN_X86_DIR" Name="x86">
                        </Directory>
                     </Directory>
                  </Directory>
               </Directory>
            </Directory>
         </Directory>
      </Directory>
      <!-- End of directory layout -->

      <!-- Components -->
      <Component Id="DIRECTORIES" Guid="F0C0B2B1-739F-4767-BBD8-FA3137AB2E66"
         Directory="APPLICATIONFOLDER">
         <CreateFolder Directory="BIN_DIR"/>
         <CreateFolder Directory="LIB_DIR"/>
         <CreateFolder Directory="LIB_X86_DIR"/>
         <CreateFolder Directory="INCLUDE_DIR"/>
         <CreateFolder Directory="CONF_DIR"/>
         <CreateFolder Directory="DATA_DIR"/>
         <CreateFolder Directory="DATA_VIDEO_DIR"/>
         <CreateFolder Directory="DATA_VIDEO_TRIUMI_DIR"/>
         <CreateFolder Directory="DATA_VIDEO_TRIUMI_SAMPLES_DIR"/>
         <CreateFolder Directory="DATA_VIDEO_TRIUMI_SAMPLES_SET1_DIR"/>
         <CreateFolder Directory="DOC_DIR"/>
         <CreateFolder Directory="DOC_API_DIR"/>
         <CreateFolder Directory="DOC_API_JAVADOC_DIR"/>
         <CreateFolder Directory="LOG_DIR"/>
         <CreateFolder Directory="TMP_DIR"/>
         <CreateFolder Directory="TEST_DIR"/>
         <CreateFolder Directory="TEST_CLASSES_DIR"/>
         <CreateFolder Directory="TEST_LIB_DIR"/>
         <CreateFolder Directory="TEST_OUTPUT_DIR"/>
      </Component>
      <Component Id="CONF_COMPONENT" Guid="0838B27B-A727-42E1-891F-574FCBEE5ED8"
         Directory="CONF_DIR">
         <File Source="@PROJECT_DIR@\target\classes\log4cxx.properties"/>
         <File Source="@PROJECT_DIR@\target\classes\logback.xml"/>
      </Component>
      <Component Id="RUNTIME_JARS" Guid="1A09247D-6B6F-4166-9B46-297520B0E735"
         Directory="LIB_DIR">
         <File Source="@MAIN_JAR_PATH@"/>
         <?foreach DEPENDENCY in @RUNTIME_CLASSPATH@?>
            <File Source="$(var.DEPENDENCY)"/>
         <?endforeach?>
      </Component>
      <Component Id="TEST_JARS" Guid="BD48DB37-90DE-4BC1-8C4B-53C3B5154A75"
         Directory="TEST_LIB_DIR">
         <File Source="@TEST_JAR_PATH@"/>
         <?foreach DEPENDENCY in @TEST_CLASSPATH@?>
            <File Source="$(var.DEPENDENCY)"/>
         <?endforeach?>
      </Component>
      <Component Id="DEPENDENT_JARS_ZIP" Guid="085DF0A0-11B3-49B2-9541-11D50F94F4C2"
         Directory="TMP_DIR">
         <File Source="@DEPENDENT_JARS_ZIP_PATH@"/>
      </Component>

      <!-- Native components -->
      <Component Id="ADAPTER_VA_SKT_DLL" Guid="C351BEBD-300D-498F-9082-055C521F4D62"
         Directory="BIN_X86_VC9_DIR">
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\@ADAPTER_VA_SKT_BUILD_CONFIG@\@ADAPTER_VA_SKT_ARTIFACT_NAME@.dll" />
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\@ADAPTER_VA_SKT_BUILD_CONFIG@\@ADAPTER_VA_SKT_ARTIFACT_NAME@.pdb" />
      </Component>
      <Component Id="ADAPTER_VA_SKT_LIB" Guid="08A3808D-C471-43EB-A0DA-13B412ADA841"
         Directory="LIB_X86_VC9_DIR">
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\@ADAPTER_VA_SKT_BUILD_CONFIG@\@ADAPTER_VA_SKT_ARTIFACT_NAME@.lib" />
      </Component>
      <Component Id="ADAPTER_VMS_TRIUMI_DLL" Guid="F281C90F-A9F9-4F7F-B02F-9D2203FA166E"
         Directory="BIN_X86_VC9_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\@ADAPTER_VMS_TRIUMI_BUILD_CONFIG@\@ADAPTER_VMS_TRIUMI_ARTIFACT_NAME@.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\@ADAPTER_VMS_TRIUMI_BUILD_CONFIG@\@ADAPTER_VMS_TRIUMI_ARTIFACT_NAME@.pdb" />
      </Component>
      <Component Id="ADAPTER_VMS_TRIUMI_LIB" Guid="35D0B232-85DD-4DF6-99B3-CE1C22EBB36C"
         Directory="LIB_X86_VC9_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\@ADAPTER_VMS_TRIUMI_BUILD_CONFIG@\@ADAPTER_VMS_TRIUMI_ARTIFACT_NAME@.lib" />
      </Component>
      <!-- End of native components -->

      <!-- 3rd-party components -->
      <Component Id="SKT_VA_DLL" Guid="715FC1A4-F271-43A1-B777-9F9E6C558E80"
         Directory="SKT_VC_BIN_X86_VC10_DIR">
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\src\nar\resources\bin\VideoAnalyticsDLL.dll" />
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\src\nar\resources\bin\AppVC.dll" />
      </Component>
      <Component Id="SKT_VA_LIB" Guid="477F4180-9755-44D1-80F2-D852637FB833"
         Directory="SKT_VC_LIB_X86_VC10_DIR">
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\src\nar\resources\lib\AppVC.lib" />
      </Component>
      <Component Id="TRIUMI_DLL" Guid="001B65A4-7750-4DF2-BCC4-D31F2F1B46C2"
         Directory="TRIUMI_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\AxInterop.MSTSCLib.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Client.Shared.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Client.Shared.pdb" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\FFH264DecoderLib.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\FFMPEGDecoderLib.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\H264DecoderLib.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\InDrawLib.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Interop.ComUtilitiesLib.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Interop.MSTSCLib.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Interop.SHDocVw.dll" />
         <File
            Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Interop.SundanceCoreLibrary.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\NxCVLib.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Sundance.Common.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Sundance.Common.pdb" />
         <File
            Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Sundance.Interop.BackupSDK.dll" />
         <File
            Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Sundance.Interop.BackupSDK.pdb" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Sundance.Interop.SDK.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Sundance.Interop.SDK.pdb" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Sundance.SharedResources.dll" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Sundance.SharedResources.pdb" />
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\SundanceFSUtils.dll" />
      </Component>
      <!-- @TODO Find out the way to merge TRIUMI_KO_DLL into TRIUMI_DLL -->
      <Component Id="TRIUMI_KO_DLL" Guid="A5364642-BAD6-45A1-9C0C-D1AA7355A0CE"
         Directory="TRIUMI_BIN_X86_KO_DIR">
         <File
            Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\ko-KR\Client.Shared.resources.dll" />
         <File
            Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\ko-KR\Sundance.Common.resources.dll" />
      </Component>
      <!-- @TODO(Done) Is Sundance.Interop.BackupSDK.lib necessary? : Deleted -->

      <Component Id="LOG4CXX_DLL" Guid="21F55756-3245-4516-A781-D30AD8814731"
         Directory="LOG4CXX_BIN_X86_VC9_DIR">
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\src\nar\resources\bin\log4cxx-0.10.0.dll" />
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\src\nar\resources\bin\log4cxx-0.10.0d.dll" />
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\src\nar\resources\bin\log4cxx-0.10.0d.pdb" />
      </Component>
      <Component Id="LOG4CXX_LIB" Guid="B3690EDF-E155-4700-BEBC-C3F7B873ADFD"
         Directory="LOG4CXX_LIB_X86_VC9_DIR">
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\src\nar\resources\lib\log4cxx-0.10.0.lib" />
         <File Source="@ADAPTER_VA_SKT_PROJECT_DIR@\src\nar\resources\lib\log4cxx-0.10.0d.lib" />
      </Component>
      <Component Id="OPENCV_249_DLL" Guid="E38DB7CB-6456-48D1-BADC-B42944916E1D"
         Directory="OPENCV_BIN_X86_VC10_DIR">
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_calib3d249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_calib3d249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_contrib249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_contrib249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_core249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_core249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_features2d249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_features2d249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_ffmpeg249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_flann249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_flann249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_gpu249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_gpu249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_highgui249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_highgui249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_imgproc249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_imgproc249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_legacy249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_legacy249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_ml249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_ml249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_nonfree249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_nonfree249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_objdetect249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_objdetect249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_ocl249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_ocl249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_photo249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_photo249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_stitching249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_stitching249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_superres249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_superres249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_video249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_video249d.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_videostab249.dll" />
         <File Source="@OPENCV_BASE@\build\x86\vc10\bin\opencv_videostab249d.dll" />
         <File Source="@OPENCV_BASE@\build\java\x86\opencv_java249.dll" />
      </Component>
      <Component Id="OPENCV_249_LIB" Guid="2D2DB742-72D8-4981-9257-D17F069633E9"
         Directory="OPENCV_LIB_X86_VC10_DIR">
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_calib3d249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_calib3d249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_contrib249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_contrib249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_core249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_core249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_features2d249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_features2d249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_flann249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_flann249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_gpu249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_gpu249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_highgui249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_highgui249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_imgproc249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_imgproc249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_legacy249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_legacy249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_ml249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_ml249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_nonfree249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_nonfree249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_objdetect249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_objdetect249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_ocl249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_ocl249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_photo249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_photo249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_stitching249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_stitching249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_superres249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_superres249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_ts249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_ts249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_video249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_video249d.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_videostab249.lib"/>
         <File Source="@OPENCV_BASE@\build\x86\vc10\lib\opencv_videostab249d.lib"/>
      </Component>
      <Component Id="EMGU_CV_242_DLL" Guid="0B514298-4516-49A4-A1F7-E1F85C2BB443"
         Directory="EMGU_CV_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Emgu.CV.dll"/>
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Emgu.CV.GPU.dll"/>
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Emgu.CV.UI.dll"/>
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Emgu.Util.dll"/>
      </Component>
      <Component Id="OPENCV_SHARP_DLL" Guid="611F55EA-EA25-4D98-9312-46ECF8BF4E3F"
         Directory="OPENCV_SHARP_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\OpenCvSharp.dll"/>
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\OpenCvSharp.Blob.dll"/>
      </Component>
      <Component Id="FFMPEG_DLL" Guid="AD1E6532-9ADB-422E-81FD-99569B78F143"
         Directory="FFMPEG_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\avcodec-54-DVR.dll"/>
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\avformat-54-DVR.dll"/>
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\avutil-51.dll"/>
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\avutil-51-DVR.dll"/>
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\swscale-2-DVR.dll"/>
      </Component>
      <Component Id="INTEL_JPEG_DLL" Guid="0E97BF57-8017-4720-A31F-368AAE56C204"
         Directory="INTEL_JPEG_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\ijl20.dll"/>
      </Component>
      <Component Id="GENUINE_CHANNELS_DLL" Guid="530EED6D-7266-444B-B496-822059C0BAE2"
         Directory="GENUINE_CHANNELS_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\GenuineChannels.dll"/>
      </Component>
      <Component Id="SMART_THREAD_POOL_DLL" Guid="FE15D05E-82C6-4476-B4BE-5E9E92E233E2"
         Directory="SMART_THREAD_POOL_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\SmartThreadPool.dll"/>
      </Component>
      <Component Id="SHARP_ZIP_DLL" Guid="19F40E7E-9152-4AC4-9326-22EA997930CD"
         Directory="SHARP_ZIP_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\ICSharpCode.SharpZipLib.dll"/>
      </Component>
      <Component Id="JSON_NET_DLL" Guid="3994E1CC-3E24-4B8D-BDF4-F7E14CA1C9BD"
         Directory="JSON_NET_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\Newtonsoft.Json.dll"/>
      </Component>
      <Component Id="VNC_SHARP_DLL" Guid="B64358AB-B030-463B-B9C5-C39411F6C2CD"
         Directory="VNC_SHARP_BIN_X86_DIR">
         <File Source="@ADAPTER_VMS_TRIUMI_PROJECT_DIR@\src\nar\resources\bin\VncSharp.dll"/>
      </Component>
      <!-- End of 3rd-party components -->
      <!-- End of components -->

      <!-- Features -->
      <Feature Id="CORE_FEATURE" Title="Core components" Level="1">
         <ComponentRef Id="DIRECTORIES"/>
         <ComponentRef Id="CONF_COMPONENT"/>
         <ComponentRef Id="RUNTIME_JARS"/>
         <ComponentRef Id="TEST_JARS"/>
         <ComponentRef Id="DEPENDENT_JARS_ZIP"/>
      </Feature>

      <Feature Id="SKT_VA_ADAPTER_FEATURE" Title="JNI Adapter for SKT VA Engine" Level="1">
         <ComponentRef Id="ADAPTER_VA_SKT_DLL" />
         <ComponentRef Id="ADAPTER_VA_SKT_LIB" />
      </Feature>
      <Feature Id="TRIUMI_VMS_ADAPTER_FEATURE" Title="JNI Adapter for Trium i VMS" Level="1">
         <ComponentRef Id="ADAPTER_VMS_TRIUMI_DLL" />
         <ComponentRef Id="ADAPTER_VMS_TRIUMI_LIB" />
      </Feature>
      <Feature Id="SKT_VA_FEATURE" Title="SKT VA Engine Core" Level="1">
         <ComponentRef Id="SKT_VA_DLL" />
         <ComponentRef Id="SKT_VA_LIB" />
      </Feature>
      <Feature Id="TRIUMI_FEATURE" Title="Trium i Interface Library" Level="1">
         <ComponentRef Id="TRIUMI_DLL" />
         <ComponentRef Id="TRIUMI_KO_DLL" />
      </Feature>
      <Feature Id="LOG4CXX_FEATURE" Title="Log4cxx 0.10 Binaries" Level="1">
         <ComponentRef Id="LOG4CXX_DLL" />
         <ComponentRef Id="LOG4CXX_LIB" />
      </Feature>
      <Feature Id="OPENCV_249_FEATURE" Title="OpenCV 2.4.9 Binaries" Level="1">
         <ComponentRef Id="OPENCV_249_DLL" />
         <ComponentRef Id="OPENCV_249_LIB" />
      </Feature>
      <Feature Id="EMGU_CV_242_FEATURE" Title="Emgu CV 2.4.2 Binaries" Level="1">
         <ComponentRef Id="EMGU_CV_242_DLL" />
      </Feature>
      <Feature Id="OPENCV_SHARP_FEATURE" Title="OpenCvSharp Binaries" Level="1">
         <ComponentRef Id="OPENCV_SHARP_DLL" />
      </Feature>
      <Feature Id="FFMPEG_FEATURE" Title="FFmpeg Binaries" Level="1">
         <ComponentRef Id="FFMPEG_DLL" />
      </Feature>
      <Feature Id="INTEL_JPEG_FEATURE" Title="Intel JPEG Library Binaries" Level="1">
         <ComponentRef Id="INTEL_JPEG_DLL" />
      </Feature>
      <Feature Id="GENUINE_CHANNELS_FEATURE" Title="Genuine Channels Binaries" Level="1">
         <ComponentRef Id="GENUINE_CHANNELS_DLL" />
      </Feature>
      <Feature Id="SMART_THREAD_POOL_FEATURE" Title="Smart Thread Pool Binaries" Level="1">
         <ComponentRef Id="SMART_THREAD_POOL_DLL" />
      </Feature>
      <Feature Id="SHARP_ZIP_FEATURE" Title="SharpZipLib Binaries" Level="1">
         <ComponentRef Id="SHARP_ZIP_DLL" />
      </Feature>
      <Feature Id="JSON_NET_FEATURE" Title="Json.NET Binaries" Level="1">
         <ComponentRef Id="JSON_NET_DLL" />
      </Feature>
      <Feature Id="VNC_SHARP_FEATURE" Title="VNC# 1.0 Binaries" Level="1">
         <ComponentRef Id="VNC_SHARP_DLL" />
      </Feature>

      <!-- UI -->
      <UI>
         <UIRef Id="WixUI_Advanced" />
      </UI>
   </Product>
</Wix>
```

Ant task to build the above source

``` xml
<target name="package.dlls.using.wix" depends="check.env"
   description="Create windows installer in .msi file for NEXCORE VAS Windows libraries(dlls, libs and so on)">
   <zip destfile="${project.pom.build.directory}/dependent-jars.zip"
        basedir="${project.pom.build.directory}/dependency"
        update="true" />

   <copy file="${basedir}/src/main/config/wix/default.wxs" tofile="${project.pom.build.directory}/wix/default.wxs" overwrite="true" force="true" filtering="true">
      <filterset>
         <filter token="VAS_VERSION" value="${version.wo.snapshot}"/>
         <filter token="PROJECT_DIR" value="${basedir}"/>
         <filter token="MAIN_JAR_PATH" value="${project.pom.build.directory}/${project.pom.build.finalName}.jar"/>
         <filter token="TEST_JAR_PATH" value="${project.pom.build.directory}/${project.pom.build.finalName}-tests.jar"/>
         <filter token="RUNTIME_CLASSPATH" value="${toString:project.classpath}"/>
         <filter token="TEST_CLASSPATH" value="${toString:project.testonly.classpath}"/>
         <filter token="DEPENDENT_JARS_ZIP_PATH" value="${project.pom.build.directory}/dependent-jars.zip"/>
         <filter token="ADAPTER_VA_SKT_PROJECT_DIR" value="${basedir}\..\SECU_ADAPTER_VA_SKT"/>
         <filter token="ADAPTER_VA_SKT_BUILD_CONFIG" value="Release"/>
         <filter token="ADAPTER_VA_SKT_ARTIFACT_NAME" value="va_skt_adapter_jni"/>
         <filter token="ADAPTER_VMS_TRIUMI_PROJECT_DIR" value="${basedir}\..\SECU_ADAPTER_VMS_TRIUMI"/>
         <filter token="ADAPTER_VMS_TRIUMI_BUILD_CONFIG" value="Release"/>
         <filter token="ADAPTER_VMS_TRIUMI_ARTIFACT_NAME" value="vms_triumi_adapter_jni"/>
         <filter token="OPENCV_BASE" value="${env.OPENCV_DIR}\..\..\.."/>
      </filterset>
   </copy>
   <schemavalidate file="${project.pom.build.directory}/wix/default.wxs"
      disableDTD="true"
      failonerror="false">
      <schema namespace="http://schemas.microsoft.com/wix/2006/wi"
         file="${project.pom.basedir}/src/main/resources/org/wixtoolset/schemas/wix.xsd"/>
   </schemavalidate>
   <exec executable="cmd" dir="${basedir}" osfamily="windows" failonerror="true">
      <arg value='/C'/>
      <arg value='${env.WIX_HOME}\\candle.exe'/>
      <arg value='-out'/>
      <arg value='${project.pom.build.directory}\\wix\\default.wixobj'/>
      <arg value='-v'/>
      <arg value='${project.pom.build.directory}\\wix\\default.wxs'/>
   </exec>
   <exec executable="cmd" dir="${basedir}" osfamily="windows" failonerror="true">
      <arg value='/C'/>
      <arg value='"${env.WIX_HOME}\\light.exe"'/>
      <arg value='-out'/>
      <arg value='${project.pom.build.directory}\\nexcore-vas-dlls.msi'/>
      <arg value='-ext'/>
      <arg value='WixUIExtension'/>
      <arg value='-ext'/>
      <arg value='WixUtilExtension'/>
      <arg value='-v'/>
      <arg value='${project.pom.build.directory}\\wix\\default.wixobj'/>
   </exec>
</target>
```
