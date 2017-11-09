---
layout: default
title: Platforms, Frameworks, Libraries and Tools
description: ...
group: software developer
toc: true
---

Platforms
---------

### SMACK

-   [Why is the SMACK stack all the rage lately?](http://www.natalinobusa.com/2015/11/why-is-smack-stack-all-rage-lately.html) (November 20, 2015)
-   [Data processing platforms architectures with SMACK: Spark, Mesos, Akka, Cassandra and Kafka](http://datastrophic.io/data-processing-platforms-architectures-with-spark-mesos-akka-cassandra-and-kafka/) (16 SEPTEMBER 2015)
-   [Streaming Analytics with Spark, Kafka, Cassandra and Akka](http://www.slideshare.net/helenaedelson/streaming-analytics-with-spark-kafka-cassandra-and-akka) (Oct 31, 2015)

<!-- -->

-   [KillrWeather](https://github.com/killrweather/killrweather)
    -   a reference application (which we are constantly improving) showing how to easily leverage and integrate Apache Spark, Apache Cassandra, and Apache Kafka for fast, streaming computations in asynchronous Akka event-driven environments.

### Vagrant

-   <https://www.vagrantup.com/>
-   Desc. : Create and configure lightweight, reproducible, and portable development environments.

### ROS

-   <http://www.ros.org/>
-   Desc. : a flexible framework for writing robot software.
-   License :

Frameworks
----------

### Networking

#### Vert.x

-   <http://vertx.io/>
-   Desc. : a tool-kit for building reactive applications on the JVM.
-   License :
-   Sources : <https://github.com/eclipse/vert.x>
-   Readings

### Testing

#### Selenium

-   <http://docs.seleniumhq.org/>
-   Desc. : automates browsers
-   License : Apache License, Version 2.0
-   Readings
    -   Official documentation : <http://docs.seleniumhq.org/docs/>
    -   [Java API](http://selenium.googlecode.com/git/docs/api/java/index.html)

#### XMLUnit

-   <http://xmlunit.sourceforge.net/>
-   Desc. : JUnit and NUnit testing for XML

### Mobile

#### Sencha Touch

-   <http://www.sencha.com/products/touch/>
-   Desc. : a high-performance HTML5 mobile application framework
-   License : GPLv3 or commercial
-   Readings
    -   [API Documentation for Sencha Touch 2.1.1](http://docs.sencha.com/touch/2-1/#!/api)
    -   [Getting Started with Sencha Touch 2.1.1](http://docs.sencha.com/touch/2-1/#!/guide/getting_started)
    -   [Sencha Cmd Guide](http://docs.sencha.com/touch/2-1/#!/guide/command)

Libraries
---------

### XML

#### EXSLT

-   <http://exslt.org/>
-   Desc. : a community initiative to provide extensions to XSLT.
-   License : ?

#### Jing

-   <https://code.google.com/p/jing-trang/>
-   Desc. : an application for validating an XML document against a RELAX NG schema, in either the XML or the compact syntax.
-   License : New BSD License
-   Readings
    -   [Jing API](http://www.thaiopensource.com/relaxng/api/jing/)

#### WSDL viewer

-   <https://code.google.com/p/wsdl-viewer/>
-   Desc. : transforms the WSDL into HTML.
-   License : New BSD License

### misc

#### Protocol Buffers

-   <https://code.google.com/p/protobuf/>
-   Desc. : a language-neutral, platform-neutral, extensible way of serializing structured data for use in communications protocols, data storage, and more.
-   License : BSD License

#### CKEditor

-   <http://ckeditor.com/>
-   Desc. : a text editor to be used inside web pages.
-   License : GPL, LGPL, MPL

#### markItUp

-   <http://markitup.jaysalvat.com/>
-   Desc. : allows you to turn any textarea into a markup editor such as Html, Textile, Wiki Syntax, Markdown, BBcode or even your own Markup system.
-   License : MIT/GPL

#### Scintilla

-   <http://www.scintilla.org/>
-   Desc. : A free source code editing component for Win32, GTK+, and OS X
-   License : permits use in any free project or commercial product.

Tools
-----

### IDE

#### NetBeans

-   <https://netbeans.org/>
-   Desc. : lets you quickly and easily develop Java desktop, mobile, and web applications, while also providing great tools for PHP and C/C++ developers.
-   License : dual license consisting of the CDDL v1.0 and the GPL v2

#### IntelliJ IDEA

-   <https://www.jetbrains.com/idea/>
-   Desc. : intelligent IDE for Java and other technologies with an exceptional out-of-the-box feature set
-   License :

### Modeling

#### Sirius

-   <http://www.eclipse.org/sirius/>
-   Desc. : an Eclipse project which allows you to easily create your own graphical modeling workbench by leveraging the Eclipse Modeling technologies, including EMF and GMF.
-   License

#### SQL Developer Data Modeler

-   <http://www.oracle.com/technetwork/developer-tools/datamodeler/overview/>
-   Desc. : create, browse and edit, logical, relational, physical, multi-dimensional, and data type models.
-   License :

<!-- -->

-   Features supported or not supported

<table>
<thead>
<tr class="header">
<th><p>Feature</p></th>
<th><p>Supporting</p></th>
<th><p>Remarks</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p>Defining AK(Alternative Key)</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Defining View</p></td>
<td><p>Yes</p></td>
<td></td>
</tr>
<tr class="odd">
<td><p>Defining FK from AK</p></td>
<td></td>
<td></td>
</tr>
<tr class="even">
<td><p>Using Domain</p></td>
<td><p>Yes</p></td>
<td><p>datatype alias for more intuitive and consistent representation</p></td>
</tr>
<tr class="odd">
<td><p>Generate DDL</p></td>
<td><p>Yes</p></td>
<td><p>- Oracle Database 9i/10g/11g/12c, SQL Server 2000/2005/2008/2012, DB2/UDB 7.1/8.1/9<br />
- Not supporting MySQL and PostgreSQL as of 4.2</p></td>
</tr>
<tr class="even">
<td><p>Using name template</p></td>
<td><p>Yes</p></td>
<td><p>PK, FK, UK, Check, Index, ...</p></td>
</tr>
</tbody>
</table>

-   Readings
    -   [How to export image of relational model in Oracle SQL Data Modeler?](http://stackoverflow.com/questions/19275848/how-to-export-image-of-relational-model-in-oracle-sql-data-modeler) (Oct 9 '13)
        -   `[File > Print Diagram > To Image File]`
    -   [Configuring Display of Model Relationships in Oracle SQL Developer Data Modeler](http://www.thatjeffsmith.com/archive/2013/01/configuring-display-of-model-relationships-in-oracle-sql-developer-data-modeler/) (January 31, 2013)
        -   font, color, ...

#### SQL Power Architect

-   <http://www.sqlpower.ca/page/architect>
-   Desc. : Data Modeling & Profiling Tool
-   License :

<!-- -->

-   Features supported or not supported

| Feature                      | Supporting | Remarks |
|------------------------------|------------|---------|
| Defining AK(Alternative Key) | Yes        |         |
| Defining View                | No         |         |
| Defining FK from AK          | No         |         |

#### UMLGraph

-   <http://www.umlgraph.org/>
-   Desc. : allows the declarative specification and drawing of UML class and sequence diagrams.
-   License : BSD license
-   Readings
    -   [User Manual](http://www.umlgraph.org/doc/indexw.html)
    -   [Javadocs and UML class diagrams with UMLGraphDoc](http://www.umlgraph.org/doc/cd-umldoc.html)

### SQL Client/Database Management

#### SQuirreL SQL Client

-   <http://www.squirrelsql.org/>
-   Desc. : A graphical Java program that will allow you to view the structure of a JDBC compliant database, browse the data in tables, issue SQL commands etc.

#### IBM Data Studio

-   <http://www-01.ibm.com/software/data/optim/data-studio/>
-   Desc. : Provides database developers and DBAs an integrated, modular environment for productive administration of DB2 for Linux, UNIX and Windows.

#### Oracle SQL Developer

-   <http://www.oracle.com/technetwork/developer-tools/sql-developer/>

#### SchemaSpy

-   <http://schemaspy.sourceforge.net/>
-   Desc. : a Java-based tool (requires Java 5 or higher) that analyzes the metadata of a schema in a database and generates a visual representation of it in a browser-displayable format.
-   License : Lesser GNU Public License 2.1

#### SQLLine

-   <http://sqlline.sourceforge.net/>
-   Desc. : a pure-Java console based utility for connecting to relational databases and executing SQL commands.

#### Robomongo

-   <http://robomongo.org/>
-   Desc. : Shell-centric cross-platform MongoDB management tool

#### HeidiSQL

-   <http://www.heidisql.com/>
-   Desc. : a useful and reliable tool designed for web developers using the popular MySQL server, Microsoft SQL databases and PostgreSQL
-   License : GPL

### Software License Analysis

#### FOSSology

-   <http://www.fossology.org/projects/fossology>
-   Desc. : scanning files for licenses and copyrights.
-   License : GPL version 2

#### Ninka

-   <http://ninka.turingmachine.org/>
-   Desc. : a lightweight license identification tool for source code
-   License : [Affero General Public License version 3.0](http://en.wikipedia.org/wiki/Affero_General_Public_License)

#### OSS Discovery

-   <http://ossdiscovery.sourceforge.net/>
-   Desc. : finds the open source software embedded in applications and installed on computers.
-   License : [GNU Affero Public License v3](http://ossdiscovery.sourceforge.net/license.html)

### System Monitoring

-   [Process supervision software and tools](http://en.wikipedia.org/wiki/Process_supervision)

#### nmon

-   <http://www.ibm.com/developerworks/wikis/display/WikiPtype/nmon>
-   Desc. : free tool giving you a huge amount of important performance information in one go.

#### top

-   <http://www.unixtop.org/>
-   Desc. : a UNIX utility that provides a rolling display of top cpu using processes.

<!-- -->

-   References
    -   [**Ubuntu manuals - `top`**](http://manpages.ubuntu.com/manpages/xenial/man1/top.1.html)
    -   [`top` man page](https://linux.die.net/man/1/top)

<!-- -->

-   Readings
    -   [How To Use The Linux Top Command To Show Running Processes](https://www.lifewire.com/linux-top-command-2201163) (February 08, 2017)
    -   [How to use the Linux top command](http://alvinalexander.com/linux/unix-linux-top-command-cpu-memory)(June 3 2016)
    -   [Why Process CPU % Usage larger than Total CPU Time](https://unix.stackexchange.com/questions/15733/why-process-cpu-usage-larger-than-total-cpu-time)(Jun 27 '11)

<!-- -->

-   Tips
    -   Remarkable commands
        -   **`W`** : Write-the-Configuration-File
        -   **`f`**` | `**`F`** : :Fields-Management
        -   **`H`** : Threads-mode toggle
        -   **`I`** : Irix/Solaris-Mode toggle
        -   **`t`** : Task/Cpu-States toggle
        -   **`m`** : Memory/Swap-Usage toggle
        -   **`1`** : Single/Separate-Cpu-States toggle
        -   **`c`** : Command-Line/Program-Name toggle
        -   **`u`**` | `**`U`** : Show-Specific-User-Only
        -   **`i`** : Idle-Process toggle

#### htop

-   <http://hisham.hm/htop/>
-   Desc. : an interactive process viewer
-   License : General Public License, version 2 (GPL-2.0)
-   Written in : C
-   Sources : <https://github.com/hishamhm/htop>

#### Nmap

-   <http://nmap.org/>
-   Desc. : a free and open source (license) utility for network discovery and security auditing.

#### Wireshark

-   <http://www.wireshark.org/>
-   Desc. : lets you capture and interactively browse the traffic running on a computer network.
-   References
    -   [Wireshark User’s Guide](https://www.wireshark.org/docs/wsug_html_chunked/)
        -   [Filtering while capturing](https://www.wireshark.org/docs/wsug_html_chunked/ChCapCaptureFilterSection.html)
        -   [**Building display filter expressions**](https://www.wireshark.org/docs/wsug_html_chunked/ChWorkBuildDisplayFilterSection.html)
    -   [Wireshark filter syntax and reference](https://www.wireshark.org/docs/man-pages/wireshark-filter.html)
    -   [CaptureFilters](https://wiki.wireshark.org/CaptureFilters)
    -   [DisplayFilters](https://wiki.wireshark.org/DisplayFilters)
    -   [Display Filter Reference](https://www.wireshark.org/docs/dfref/)

<!-- -->

-   Readings
    -   [How To Set Up a Capture](https://wiki.wireshark.org/CaptureSetup)
    -   [**Loopback capture setup**](https://wiki.wireshark.org/CaptureSetup/Loopback)
    -   [Wireshark basics 101: A simple concise tutorial for beginners](https://www.concise-courses.com/security/wireshark-basics/) (August 17, 2013)
    -   [How to Use Wireshark to Capture, Filter and Inspect Packets](http://www.howtogeek.com/104278/how-to-use-wireshark-to-capture-filter-and-inspect-packets/)
    -   [Wireshark: A Guide to Color My Packets](http://www.sans.org/reading-room/whitepapers/detection/wireshark-guide-color-packets-35272) (1st July 2014)
    -   [Getting Started with Wireshark](http://www.ni.com/white-paper/6746/en/) (11, 07, 2014)
    -   [Let me tell you about Wireshark 2.0](https://blog.wireshark.org/2015/11/let-me-tell-you-about-wireshark-2-0/) (November 6, 2015)

<!-- -->

-   Typical display filters

``` text
ip.src == 192.168.1.31 and ip.addr == 203.252.150.28 and http
```

-   Typical capture filters

#### iperf

-   <http://software.es.net/iperf/>
-   Desc. : a tool for active measurements of the maximum achievable bandwidth on IP networks.
-   License : BSD
-   Readings
    -   [`iperf` at Wikipedia](https://en.wikipedia.org/wiki/Iperf)

#### Process Explorer

-   <https://technet.microsoft.com/en-us/library/bb896653.aspx>
-   Desc. : shows you information about which handles and DLLs processes have opened or loaded.

#### Process Monitor

-   <https://technet.microsoft.com/en-us/library/bb896645.aspx>
-   Desc. : an advanced monitoring tool for Windows that shows real-time file system, Registry and process/thread activity.

#### DebugView

-   <https://technet.microsoft.com/en-us/sysinternals/debugview.aspx>
-   Desc. : an application that lets you monitor debug output on your local system, or any computer on the network that you can reach via TCP/IP.

### Testing

#### JMeter

-   <http://jmeter.apache.org/>
-   Des. : a 100% pure Java application designed to load test functional behavior and measure performance.
-   License : Apache License v2

<!-- -->

-   References
    -   [User's Manual](http://jmeter.apache.org/usermanual/index.html)
        -   [Elements of a Test Plan](http://jmeter.apache.org/usermanual/test_plan.html)
    -   [Component Reference](http://jmeter.apache.org/usermanual/component_reference.html)
        -   [User Defined Variables](http://jmeter.apache.org/usermanual/component_reference.html#User_Defined_Variables)
        -   [User Parameters](http://jmeter.apache.org/usermanual/component_reference.html#User_Parameters)
        -   [CSV Data Set Config](http://jmeter.apache.org/usermanual/component_reference.html#CSV_Data_Set_Config)
        -   [Response Assertion](http://jmeter.apache.org/usermanual/component_reference.html#Response_Assertion)
    -   [Functions and Variables](http://jmeter.apache.org/usermanual/functions.html)
        -   [Pre-defined Variables](http://jmeter.apache.org/usermanual/functions.html#predefinedvars)

<!-- -->

-   Readings
    -   [Free Jmeter Tutorial](http://www.guru99.com/jmeter-tutorials.html)
        -   [**Complete Element reference for Jmeter**](http://www.guru99.com/jmeter-element-reference.html)
    -   [Using User Defined Variables](https://guide.blazemeter.com/hc/en-us/articles/207421395-Using-User-Defined-Variables)
    -   [Use request value from list of values in JMeter](http://stackoverflow.com/questions/6341600/use-request-value-from-list-of-values-in-jmeter) (Jun 14 '11)
    -   [JMeter tips](http://www.javaworld.com/article/2071953/testing-debugging/jmeter-tips.html) (JUL 11, 2005)

<!-- -->

-   Companions
    -   [Custom Plugins for Apache JMeter](https://jmeter-plugins.org/)

##### Tips and Trics

###### Using variables in assertion

In 'Response Assertion', the variable defined via 'CSV Data Set Config' or 'User Defined Variables' can be used in test pattern.
In the following sample, `certHash` is variable defined in 'CSV Data Set Config'

``` bash
${__escapeOroRegexpChars(\"cert_hash\":\"${certHash}\")}
```

#### Eclipse TPTP

-   <http://www.eclipse.org/tptp/>
-   Desc. : provides an open platform supplying powerful frameworks and services that allow software developers to build unique test and performance tools, both open source and commercial, that can be easily integrated with the platform and with other tools.

#### Testopia

-   <http://www.mozilla.org/projects/testopia/>
-   Desc. : A test case management extension for Bugzilla.

#### TestLink

-   <http://sourceforge.net/projects/testlink/>
-   Desc. : a web based Test Management tool.
-   License : GPLv2

#### FitNesse

-   <http://fitnesse.org/>
-   Desc. : The fully integrated standalone wiki and acceptance testing framework.

#### CodePro Analytix

-   <https://developers.google.com/java-dev-tools/codepro/doc/>
-   Desc. : the premier Java software testing tool for Eclipse developers who are concerned about improving software quality and reducing developments costs and schedules.
-   License :

#### Postman

-   <https://www.getpostman.com/>
-   Desc. : Build, test, and document your APIs faster.
-   Readings
    -   [Setting up an environment with variables](https://www.getpostman.com/docs/environments)
    -   [Pre Request Scripts](https://www.getpostman.com/docs/pre_request_scripts)

### Log Viewer

#### Lilith

-   <http://lilith.huxhorn.de/>
-   Desc. : Lilith is a logging and access event viewer for the Logback logging framework, log4j and java.util.logging.
-   License :

### Documentation

-   [Rest Api Documentation](http://mestachs.wordpress.com/2012/08/06/rest-api-documentation/)

#### Swagger

-   <http://swagger.io/>
-   Desc. : a powerful open source framework backed by a large ecosystem of tools that helps you design, build, document, and consume your RESTful APIs
-   License :
-   Sources : <https://github.com/swagger-api>

<!-- -->

-   References
    -   [**OpenAPI Specification Ver. 2.0** (fka Swagger RESTful API Documentation Specification)](https://github.com/OAI/OpenAPI-Specification/blob/master/versions/2.0.md)
    -   [OpenAPI Spec Schema 2.0](https://github.com/OAI/OpenAPI-Specification/blob/master/schemas/v2.0/schema.json)
    -   [Swagger-Core Annotations](https://github.com/swagger-api/swagger-core/wiki/Annotations-1.5.X)
    -   [Swagger-Core Annotations API](http://docs.swagger.io/swagger-core/current/apidocs/) (Javadoc)
    -   [Swagger Code Generator](https://github.com/swagger-api/swagger-codegen)
        -   allows generation of API client libraries (SDK generation), server stubs and documentation automatically given an OpenAPI Spec
    -   [Swagger Code Generators](https://github.com/swagger-api/swagger-codegen/tree/v2.2.2/modules/swagger-codegen/src/main/java/io/swagger/codegen/languages)
        -   `SwaggerGenerator`, `StaticHtml2Generator`, `JMeterCodegen`, ...

<!-- -->

-   Readings
    -   [**Writing OpenAPI (Swagger) Specification Tutorial**](https://apihandyman.io/writing-openapi-swagger-specification-tutorial-part-1-introduction/)
    -   [How to split a Swagger spec into smaller files](http://azimi.me/2015/07/16/split-swagger-into-smaller-files.html) (Jul 16, 2015)
    -   [**Generating Swagger spec and enabling swagger-ui for REST based applications**](https://tech.homeaway.com/development/2016/06/02/generating-swagger-spec.html) (Jun 2, 2016)
    -   [Using Javadocs to generate Swagger document](http://stackoverflow.com/questions/32340735/using-javadocs-to-generate-swagger-document)
    -   [Spring Rest API with Swagger – Fine-tuning exposed documentation](https://jakubstas.com/spring-jersey-swagger-fine-tuning-exposed-documentation/) (23.11.2014)

<!-- -->

-   Examples
    -   [Swagger + SpringFox + SpringBoot example](https://github.com/swagger-api/swagger-samples/tree/master/java/java-spring-boot)

#### Swagger2Markup

-   <https://github.com/Swagger2Markup/swagger2markup>
-   Desc. : simplify the generation of an up-to-date RESTful API documentation by combining documentation that’s been hand-written with auto-generated API documentation produced by Swagger.
-   License : Apache License Version 2.0

<!-- -->

-   Readings
    -   [Swagger2Markup 1.3.1 Documentation](http://swagger2markup.github.io/swagger2markup/1.3.1/)
        -   [Swagger2Markup Maven Plugin](http://swagger2markup.github.io/swagger2markup/1.3.1/#_maven_plugin)
    -   [**Create HTML documentation from Swagger via Maven**](http://ics.upjs.sk/~novotnyr/blog/2156/create-html-documentation-from-swagger-via-maven) (Jún 29, 2016)
        -   using both Swagger2Markup Maven Plugin and Asciidoctor Maven Plugin
        -   [Asciidoctor Maven Plugin](https://github.com/asciidoctor/asciidoctor-maven-plugin)
    -   [Having fun with REST API documentation](http://planet.jboss.org/post/having_fun_with_rest_api_documentation) (Sep 22, 2015)
        -   using both Swagger2Markup Maven Plugin and Asciidoctor Maven Plugin
        -   [`org.asciidoctor.Attributes` source](https://github.com/asciidoctor/asciidoctorj/blob/master/asciidoctorj-core/src/main/java/org/asciidoctor/Attributes.java)
    -   [Swagger2Markup Maven template project](https://github.com/Swagger2Markup/swagger2markup-maven-project-template)

#### SpringFox

-   <https://springfox.github.io/springfox/>
-   Desc. : Automated JSON API documentation for API's built with Spring
-   License :
-   Sources : <https://github.com/springfox/springfox>

<!-- -->

-   Readings
    -   [Springfox Reference Documentation](http://springfox.github.io/springfox/docs/current/)
    -   [SWAGGER 2 INTEGRATION WITH SPRING REST](https://indrabasak.wordpress.com/2016/04/07/swagger-2-integration-with-spring-rest/) (April 7, 2016)

<!-- -->

-   Samples
    -   [REST Springfox/Swagger Example](https://github.com/indrabasak/rest-springfox)

#### Enunciate

-   <http://enunciate.webcohesion.com/>
-   Desc. : an engine for enhancing your Java Web service API

#### Doxygen

-   <http://www.stack.nl/~dimitri/doxygen/>
-   Desc. : a documentation system for C++, C, Java, Objective-C, Python, IDL (Corba and Microsoft flavors), Fortran, VHDL, PHP, C\#, and to some extent D.
-   License : GPL

<!-- -->

-   Readings
    -   [Official documentation](http://www.stack.nl/~dimitri/doxygen/manual/)
        -   [Documenting the code](http://www.stack.nl/~dimitri/doxygen/manual/docblocks.html)
            -   [Putting documentation after members](http://www.stack.nl/~dimitri/doxygen/manual/docblocks.html#memberdoc)
        -   [Configuration](http://www.stack.nl/~dimitri/doxygen/manual/config.html)
        -   [Special Commands](http://www.stack.nl/~dimitri/doxygen/manual/commands.html)(Tags of Javadoc)
    -   [5 Best Eclipse Plugins: \#1 (Eclox with Doxygen, Graphviz and Mscgen)](http://mcuoneclipse.com/2012/06/25/5-best-eclipse-plugins-1-eclox-with-doxygen-graphviz-and-mscgen/)

#### Sphinx

-   <http://sphinx-doc.org/>
-   Desc. :Python documentation generator
-   License : BSD license

#### Publican

-   <https://fedorahosted.org/publican/>
-   Desc. :A tool for publishing material authored in DocBook XML.

#### Pandoc

-   <http://johnmacfarlane.net/pandoc/>
-   Desc. : convert documents in markdown, reStructuredText, textile, HTML, DocBook, or LaTeX to HTML formats, word processor formats, documentation formats, TeX formats, PDF, and/or lightweight markup formats.
-   License : GPL
-   Sources

<!-- -->

-   Readings
    -   [Installing pandoc](http://pandoc.org/installing.html)
    -   [Pandoc User’s Guide](http://pandoc.org/MANUAL.html)

#### Jekyll

-   <https://github.com/mojombo/jekyll>
-   Desc. : a parsing engine bundled as a ruby gem used to build static websites from dynamic components such as templates, partials, liquid code, markdown, etc.
-   License : MIT License
-   Readings
    -   [Directory structure](http://jekyllrb.com/docs/structure/)
-   Samples
    -   [The repository of the website expressjs.com](https://github.com/strongloop/expressjs.com)

#### jax-doclets

-   <http://fromage.github.io/jax-doclets/>
-   Desc. : contains doclets for modern Java APIs such as JAXB, JAX-RS and JPA.
-   License : LGPL
-   Maven artifacts : [com.lunatech.jax-doclets » doclets](http://mvnrepository.com/artifact/com.lunatech.jax-doclets/doclets)
-   Sources : <https://github.com/FroMage/jax-doclets>
-   Readings
    -   [Official documentation for jax-doclets 0.10.0](http://fromage.github.io/jax-doclets/docs/0.10.0/html/)

#### xs3p

-   <http://sourceforge.net/projects/xs3p/>
-   Desc. : an XSLT stylesheet that will generate an XHTML document from an XSD schema.
-   License : DSTC Public License (DPL)
-   Maven artifacts
-   Sources
-   Readings
    -   [Wiki](http://wiki.fiforms.org/index.php/Xs3p)

#### Enunciate

-   <http://enunciate.codehaus.org/>
-   Desc. : an engine for dramatically enhancing your Java Web service API.
-   License : Apache License Version 2.0

#### MireDot

-   <http://www.miredot.com/>
-   Desc. : REST API documentation generator for Java that generates beautiful interactive documentation with minimal configuration effort.
-   License : complex -&gt; <http://www.miredot.com/price/>

#### Swagger Core

-   <https://github.com/wordnik/swagger-core>
-   Desc. : Java/Scala implementation of Swagger
-   License : the Apache License, Version 2.0
-   Readings
    -   [The Swagger Specification](https://github.com/wordnik/swagger-spec)

### Text Editor

#### vi

-   Readings
    -   [Basic vi Commands](https://www.cs.colostate.edu/helpdocs/vi.html)
    -   [Vi Cheat Sheet](http://ryanstutorials.net/linuxtutorial/cheatsheetvi.php)
    -   [vi Manual by Tony Chen](http://www.cs.fsu.edu/general/vimanual.html)
    -   [Split screen in vi](http://www.microshell.com/programming/quick-tips/split-screen-in-vi/) (February 17th, 2009)

#### jEdit

-   <http://www.jedit.org/>
-   Desc. : a mature programmer's text editor with hundreds (counting the time developing plugins) of person-years of development behind it
-   License : GPL 2.0

### Build

#### Maven

-   <http://maven.apache.org/>
-   Desc. : a software project management and comprehension tool.
-   [On Maven](On_Maven "wikilink")

#### Ant

-   <http://ant.apache.org/>
-   Desc. : a Java library and command-line tool whose mission is to drive processes described in build files as targets and extension points dependent upon each other.
-   [On Ant](On_Ant "wikilink")

#### Gradle

-   <http://www.gradle.org/>
-   Desc. : automate the building, testing, publishing, deployment and more of software packages or other types of projects such as generated static websites, generated documentation or indeed anything else.
-   License :
-   Source :
-   Readings
    -   [Gradle documentation](http://www.gradle.org/documentation)

### Package Management

#### RPM

-   <http://rpm.org/>
-   Desc. : a powerful command line driven package management system capable of installing, uninstalling, verifying, querying, and updating computer software packages.

<!-- -->

-   References
    -   [Fedora RPM Guide](http://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/index.html)
        -   [RPM Query](http://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/ch-using-rpm-db.html)
        -   [Spec File Syntax](http://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/ch-specfile-syntax.html)
    -   [Maximum RPM](http://www.rpm.org/max-rpm/)
        -   [Scripts: RPM's Workhorse](http://www.rpm.org/max-rpm/s1-rpm-inside-scripts.html)

<!-- -->

-   Readings
    -   [Valid RPM GROUPS](http://fedoraproject.org/wiki/RPMGroups)
    -   [How can I remove a package? Is it going to remove any dependencies?](http://fedoranews.org/alex/tutorial/rpm/2.shtml)
    -   [Test the installation or upgrade process](http://docs.fedoraproject.org/en-US/Fedora_Draft_Documentation/0.1/html/RPM_Guide/ch03s02.html#id836604)
    -   [List files in an RPM package](http://www.tech-recipes.com/rx/1419/list-files-in-an-rpm-package/)

<!-- -->

-   Example
    -   **`$ rpm -Uhv mysql-workbench-community-6.2.3-1.el6.x86_64.rpm`** `//install or upgrade. (do NOT use rpm -i ... as possible)`
    -   **`$ rpm -ql mysql-workbench-community`** `//list files in a installed package`
    -   **`$ rpm -qlp mysql-workbench-community-6.2.3-1.el6.x86_64.rpm`** `//list files in a rpm file`
    -   **`$ rpm -qp --scripts mysql-workbench-community-6.2.3-1.el6.x86_64.rpm`** `//list install/erase scriptlets in a rpm file`

#### yum

-   <http://yum.baseurl.org/>
-   Desc. : an automatic updater and package installer/remover for rpm systems.
-   Readings
    -   [Fedora Yum Guide](https://docs.fedoraproject.org/en-US/Fedora/15/html/Deployment_Guide/ch-yum.html)
    -   [Managing Software with `yum`](https://www.centos.org/docs/5/html/yum/)
    -   [Managing Packages With Yum](http://prefetch.net/articles/yum.html)
        -   `yum list, yum install, yum remove, yum update, yum check-update, yum info, yum search, yum provides, yum clean`
    -   [Basic Yum Commands and how to use them](http://yum.baseurl.org/wiki/YumCommands)
    -   [Yum groups and repositories](http://yum.baseurl.org/wiki/YumGroups)
        -   `yum grouplist, yum groupinfo, yum groupinstall, yum groupremove, yum groupupdate`
    -   [How To Download a RPM Package Using yum Command Without Installing On Linux](http://www.cyberciti.biz/faq/yum-downloadonly-plugin/) (JULY 9, 2008)
    -   [Listing Files Contained in a Package](https://access.redhat.com/site/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Displaying_Package_Information.html#bh-Listing_files_contained_in_a_package)
    -   [CentOS / RHEL: List All Configured Repositories](http://www.cyberciti.biz/faq/centos-fedora-redhat-yum-repolist-command-tolist-package-repositories/) (FEBRUARY 25, 2013)
    -   [Adding, Enabling, and Disabling a Yum Repository](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Managing_Yum_Repositories.html)
    -   [Using RPMforge](http://repoforge.org/use/)
    -   [Yum install maven : Yes you can !](http://blog.gluster.org/2013/08/yum-install-maven-yes-you-can/) (August 21, 2013)

<!-- -->

-   Commands
    -   **`list`**
        -   [Fedora Yum Guide &gt; Listing Packages](https://access.redhat.com/documentation/en-US/Red_Hat_Enterprise_Linux/6/html/Deployment_Guide/sec-Listing_Packages.html)
    -   **`provides`**
        -   [Managing Software with `yum` &gt; Advanced Searches](https://www.centos.org/docs/5/html/yum/sn-searching-packages.html#sn-searching-packages-advanced)

<!-- -->

-   Examples
    -   **`yum -v repolist enabled`** `//list only enabled repositories`
    -   **`yum -v list all subversion --enablerepo=rpmforge-extras`** `//search subversion packages both installed and available including 'rpmforge-extras' repository`
    -   '''`yum -v list installed` `//list all installed packages`

<!-- -->

-   Repositories
    -   [Extra Packages for Enterprise Linux (EPEL)](http://fedoraproject.org/wiki/EPEL)
    -   <https://repos.fedorapeople.org/>

#### APT

-   <https://wiki.debian.org/Apt>
-   Desc. : a free software user interface that works with core libraries to handle the installation and removal of software on the Debian, Slackware and other Linux distributions
-   Readings
    -   [**APT CLI**](https://wiki.debian.org/AptCLI#Installing_Packages)
    -   [APT User's Guide](https://www.debian.org/doc/manuals/apt-guide/index.en.html)

#### RubyGems

-   <http://rubygems.org/>
-   Desc. : allows you to easily download, install, and use ruby software packages on your system
-   License : Ruby License
-   Written in : Ruby
-   Readings
    -   [RubyGems at Wikipedia](http://en.wikipedia.org/wiki/RubyGems)
    -   [RubyGems Basics](http://guides.rubygems.org/rubygems-basics/)

### Frontend

#### MySchedule

-   <http://code.google.com/p/myschedule/>
-   Desc. : a web application for managing Quartz Schedulers.

### Networking

#### OpenSSH

-   <http://www.openssh.org/>
-   Desc. : a FREE version of the SSH connectivity tools that technical users of the Internet rely on.

<!-- -->

-   Readings
    -   [Documentation for OpenSSH on Fedora 17](http://docs.fedoraproject.org/en-US/Fedora/17/html/System_Administrators_Guide/ch-OpenSSH.html)
    -   [Documentation for OpenSSH on CentOS 5.2](https://www.centos.org/docs/5/html/5.2/Deployment_Guide/ch-openssh.html)
    -   [OpenSSH on Ubuntu 16.04](https://help.ubuntu.com/lts/serverguide/openssh-server.html)
    -   [`sshd_config` Man Page](https://www.freebsd.org/cgi/man.cgi?query=sshd_config&sektion=5)

<!-- -->

-   Readings
    -   [**SSH Essentials: Working with SSH Servers, Clients, and Keys**](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys) (October 16, 2014)
    -   [Improving the security of your SSH private key files](http://martin.kleppmann.com/2013/05/24/improving-security-of-ssh-private-keys.html)
    -   [How to configure and use OpenSSH on CentOS 7](https://www.rosehosting.com/blog/how-to-configure-and-use-openssh-on-centos-7/) (JULY 13, 2016)

#### OpenSSL

-   <https://www.openssl.org/>
-   Desc. : a robust, commercial-grade, full-featured, and Open Source toolkit implementing the Secure Sockets Layer (SSL v2/v3) and Transport Layer Security (TLS v1) protocols as well as a full-strength general purpose cryptography library.
-   License :
-   Readings
    -   [An Introduction to the OpenSSL command line tool](http://users.dcc.uchile.cl/~pcamacho/tutorial/crypto/openssl/openssl_intro.html)
    -   [OpenSSL Command-Line HOWTO](http://www.madboa.com/geek/openssl/)

#### curl

-   <http://curl.haxx.se/>
-   Desc. : a command line tool and library for transferring data with URL syntax, supporting DICT, FILE, FTP, FTPS, Gopher, HTTP, HTTPS, IMAP, IMAPS, LDAP, LDAPS, POP3, POP3S, RTMP, RTSP, SCP, SFTP, SMTP, SMTPS, Telnet and TFTP.
-   License :

<!-- -->

-   Readings
    -   [Documentation](http://curl.haxx.se/docs/)
    -   [Usage explained](http://curl.haxx.se/docs/manual.html)
    -   [man page](http://curl.haxx.se/docs/manpage.html)

#### PuTTY

-   <http://www.putty.org/>
-   Desc. : an SSH and telnet client, developed originally by Simon Tatham for the Windows platform.

<!-- -->

-   Readings
    -   [PuTTY 0.68 User Manual](https://the.earth.li/~sgtatham/putty/0.68/htmldoc/)
        -   [Copying and pasting text](https://the.earth.li/~sgtatham/putty/0.68/htmldoc/Chapter3.html#using-selection)
    -   [How to make PuTTY settings persistent?](https://serverfault.com/questions/12295/how-to-make-putty-settings-persistent) (May 26 '09)
    -   [Putty title changes after login](https://superuser.com/questions/510900/putty-title-changes-after-login) (Nov 26 '12)
        -   \[Terminal\] -&gt; \[Features\] -&gt; \[Disable remote-controlled window title changing\]
    -   [puTTY SSH connections dropping when on wifi instead of ethernet](https://eedle.com/2013/11/07/putty-ssh-connections-dropping-when-on-wifi-instead-of-ethernet/)
        -   \[Connection\] -&gt; \[Enable TCP keepalives (SO\_KEEPALIVE option)\]

##### Companions

###### PuTTY Session Manager

-   <https://puttysm.sourceforge.io/>
-   Desc. : a tool that allows system adminstrators to organise their PuTTY sessions into folders and assign hotkeys to their favourite sessions.

###### ConEmu

-   <http://conemu.github.io/>
-   Desc. : a Windows console emulator with tabs, which presents multiple consoles and simple GUI applications as one customizable GUI window with various features.

###### Pageant

-   [Pageant – SSH agent for Windows](https://sachinsharm.wordpress.com/2015/05/04/pageant-ssh-agent-for-windows/)
-   [Using Pageant for Authentication](https://winscp.net/eng/docs/ui_pageant)

### Graphics

#### Graphviz

-   <http://www.graphviz.org/>
-   Desc. : open source graph visualization software.
-   License : EPL

#### Mscgen

-   <http://www.mcternan.me.uk/mscgen/>
-   Desc. : a small program that parses Message Sequence Chart descriptions and produces PNG, SVG, EPS or server side image maps (ismaps) as the output.
-   License : GPLv2

#### OpenSG

-   <http://www.opensg.org/>
-   Desc. : a portable scenegraph system to create realtime graphics programs.
-   License : LGPL

#### OpenSceneGraph

-   <http://www.openscenegraph.org/projects/osg>
-   Desc. : an open source high performance 3D graphics toolkit, used by application developers in fields such as visual simulation, games, virtual reality, scientific visualization and modelling. Written entirely in Standard C++ and OpenGL.
-   License : LGPL

### Visualization

##### XsdVi

-   <http://sourceforge.net/projects/xsdvi/>
-   Desc. : transform W3C XML Schema instances into interactive diagrams in SVG format.
-   License : GPLv2

### Data Analysis

#### Cube

-   <http://square.github.com/cube/>
-   Desc. : a system for collecting timestamped events and deriving metrics.
-   License : Apache License 2.0

### ETL

#### Scriptella

-   <http://scriptella.javaforge.com/>
-   Desc. : An open source ETL (Extract-Transform-Load) and script execution tool written in Java.

Servers or Engines
------------------

### Hypervisor

#### VirtualBox

-   <https://www.virtualbox.org/>
-   Desc : a powerful x86 and AMD64/Intel64 virtualization product for enterprise as well as home use.
-   License : GPL version 2
-   References
    -   [User Manual](https://www.virtualbox.org/manual/UserManual.html)
    -   [`VBoxManage` reference](https://www.virtualbox.org/manual/ch08.html) : VirtualBox CLI
-   Readings
    -   [Install Ubuntu on Oracle VirtualBox](https://brb.nci.nih.gov/bdge/installUbuntu.html)
    -   [CentOS as a Guest OS in VirtualBox](http://wiki.centos.org/HowTos/Virtualization/VirtualBox/CentOSguest)
    -   [Install a CentOS 7 Minimal Virtual Machine with VirtualBox](http://www.jeramysingleton.com/install-centos-7-minimal-in-virtualbox/)
    -   [Using a raw host hard disk from a guest](http://www.virtualbox.org/manual/ch09.html#rawdisk)
    -   [Four virtual disk formats every VDI admin needs to know](http://searchvirtualdesktop.techtarget.com/tip/Four-virtual-disk-formats-every-VDI-admin-needs-to-know)
    -   [How to create and start VirtualBox VM without GUI](http://xmodulo.com/how-to-create-and-start-virtualbox-vm-without-gui.html) (May 9, 2013)

### DBMS

#### HSQLDB

-   <http://hsqldb.org/>
-   Desc. : the leading SQL relational database software written in Java.
-   License : BSD License
-   Maven artifacts
    -   [hsqldb » hsqldb](http://mvnrepository.com/artifact/hsqldb/hsqldb) : 1.6.x ~ 1.8.x
    -   [org.hsqldb](http://mvnrepository.com/artifact/org.hsqldb) : 2.0.x ~
-   Source : <http://sourceforge.net/p/hsqldb/svn/HEAD/tree/>
-   References
    -   [HyperSQL 2.x User Guide](http://hsqldb.org/doc/2.0/guide/index.html)
    -   [HyperSQL 1.8 User Guide](http://www.hsqldb.org/doc/1.8/guide/)
        -   [SQL Syntax](http://www.hsqldb.org/doc/1.8/guide/ch09.html)
    -   [HyperSQL 2.x API](http://www.hsqldb.org/doc/2.0/apidocs/)
    -   [`org.hsqldb.server.Server` API](http://www.hsqldb.org/doc/2.0/apidocs/index.html?org/hsqldb/server/Server.html)
        -   shows command line options and format of `server.properties` file
-   Readings
    -   [**`INFORMATION_SCHEMA`** of HSQDB 1.8](https://forum.openoffice.org/en/forum/viewtopic.php?f=13&t=64473#p286191)

#### Apache Derby

-   <http://db.apache.org/derby/>
-   Desc. : an open source relational database implemented entirely in Java
-   License : Apache License, Version 2.0
-   Readings
    -   “Official Documentation”:<http://db.apache.org/derby/manuals/index.html>
    -   “Using `ij`”:<https://builds.apache.org/job/Derby-docs/lastSuccessfulBuild/artifact/trunk/out/tools/ctoolsij34525.html>

### J2EE Application Server

#### Jetty

-   <http://jetty.codehaus.org/>
-   Desc : provides an HTTP server, HTTP client, and javax.servlet container.
-   License : <http://www.eclipse.org/jetty/licenses.php>
-   Source
    -   [Jetty: primary project repository](http://git.eclipse.org/c/jetty/org.eclipse.jetty.project.git)
    -   [Jetty Ant tasks v8.1.11 sources](http://git.codehaus.org/gitweb.cgi?p=jetty-project.git;a=tree;f=jetty-ant;h=efebaf2fbe3dd87868513206a9d806487112720d;hb=86a3fc50a131b7ed1028c8719690dc7339dce736)
    -   [`jetty.xml` and `jetty-*.xml` of Jetty 7](http://git.eclipse.org/c/jetty/org.eclipse.jetty.project.git/tree/jetty-server/src/main/config/etc?h=release-7)
    -   [`jetty-jmx.xml` of Jetty 7](http://git.eclipse.org/c/jetty/org.eclipse.jetty.project.git/tree/jetty-jmx/src/main/config/etc/jetty-jmx.xml?h=release-7)
-   Binaries
    -   [Maven Group : org.eclipse.jetty](http://mvnrepository.com/artifact/org.eclipse.jetty)
    -   [Maven Group : org.mortbay.jetty](http://mvnrepository.com/artifact/org.mortbay.jetty)
-   Documentations
    -   [Jetty Versions](http://www.eclipse.org/jetty/documentation/current/what-jetty-version.html)
    -   Jetty 9
        -   [Jetty 9 Documentation](http://www.eclipse.org/jetty/documentation/current/)
        -   [Jetty 9 Java API](http://download.eclipse.org/jetty/stable-9/apidocs/)
    -   Jetty 7/8
        -   [Jetty 7/8 Documentation](http://wiki.eclipse.org/Jetty)
        -   [Jetty 7 Java API](http://download.eclipse.org/jetty/stable-7/apidocs/)
    -   Jetty 6
        -   [Jetty 6 Documentation](http://docs.codehaus.org/display/JETTY/Jetty%20Documentation)
        -   [How to Run Jetty](http://wiki.eclipse.org/Jetty/Howto/Run_Jetty)
-   Articles
    -   Jetty 9
        -   [Embedded Examples/Web Application with JSP](http://www.eclipse.org/jetty/documentation/current/embedded-examples.html#embedded-webapp-jsp)
        -   [Example: Embedded Jetty w/ JSP Support](https://github.com/jetty-project/embedded-jetty-jsp)
        -   [Using Spring to Configure Jetty](http://www.eclipse.org/jetty/documentation/current/frameworks.html#framework-jetty-spring)
            -   <bean id="server" name="Main" class="org.eclipse.jetty.server.Server" init-method="start" destroy-method="stop">
        -   [Jetty Logging](http://www.eclipse.org/jetty/documentation/current/configuring-logging.html)
        -   [JMX](http://www.eclipse.org/jetty/documentation/current/jmx-chapter.html)
        -   [How do you get embedded Jetty 9 to successfully resolve the JSTL URI?](http://stackoverflow.com/questions/17685330/how-do-you-get-embedded-jetty-9-to-successfully-resolve-the-jstl-uri)(Jul 16 '13)
    -   Jetty 7/8
        -   [`jetty.xml` syntax](http://wiki.eclipse.org/Jetty/Reference/jetty.xml_syntax)
        -   [Embedding Jetty](http://wiki.eclipse.org/Jetty/Tutorial/Embedding_Jetty)
        -   [Jetty/Howto/Secure Termination](http://wiki.eclipse.org/Jetty/Howto/Secure_Termination)
        -   [Jetty/Tutorial/JMX](http://wiki.eclipse.org/Jetty/Tutorial/JMX)
        -   [Deal with Locked Windows Files](http://wiki.eclipse.org/Jetty/Howto/Deal_with_Locked_Windows_Files)
    -   [How to make Jetty dynamically load “static” pages](http://goodjava2.appspot.com/question/5080e7514f1eba38a4ba772a)
    -   [Debugging with the Maven Jetty Plugin inside Eclipse](http://docs.codehaus.org/display/JETTY/Debugging+with+the+Maven+Jetty+Plugin+inside+Eclipse)
    -   [Avoiding Jetty Locking Static Files](http://kaanmutlu.wordpress.com/2012/01/10/avoiding-jetty-locking-static-files/)(January 10, 2012)
    -   [Jetty Cross Origin Filter](http://stackoverflow.com/questions/8303162/jetty-cross-origin-filter) (Nov 28 '11)
        -   [`org.eclipse.jetty.servlets.CrossOriginFilter` API](http://download.eclipse.org/jetty/stable-7/apidocs/index.html?org/eclipse/jetty/servlets/CrossOriginFilter.html)

#### Tomcat

-   <http://tomcat.apache.org/>
-   Desc : an open source software implementation of the Java Servlet and JavaServer Pages technologies.
-   License : Apache License version 2
-   References
    -   [Apache Tomcat Versions](http://tomcat.apache.org/whichversion.html)
    -   [Official documentation of Apache Tomcat 7](http://tomcat.apache.org/tomcat-7.0-doc/)
    -   [Official documentation of Apache Tomcat 6](http://tomcat.apache.org/tomcat-6.0-doc/)
    -   [Official documentation of Apache Tomcat 5.5](http://tomcat.apache.org/tomcat-5.5-doc/)

#### JBoss AS

-   Wiki : <https://community.jboss.org/wiki/>

<!-- -->

-   References
    -   [JBoss Application Server Official Documentation](https://community.jboss.org/wiki/JBossApplicationServerOfficialDocumentationPage)
    -   [JBoss AS 5 Installation And Getting Started Guide](http://docs.jboss.org/jbossas/docs/Installation_And_Getting_Started_Guide/5/html/)
    -   [JBoss AS 5 Administration And Configuration Guide](http://docs.jboss.org/jbossas/docs/Administration_And_Configuration_Guide/5/html/)
    -   [JBoss AS 5 Admin Console Quick Start](https://community.jboss.org/wiki/AdminConsoleQuickStart)

<!-- -->

-   Articles
    -   **fundamental**
        -   [Startup parameter](http://community.jboss.org/wiki/JBossRunParameters)
        -   [Enabling remote access to JBoss 4.2](http://jlorenzen.blogspot.com/2007/10/enabling-remote-access-to-jboss-42.html)
        -   [Class loading configuration](http://community.jboss.org/wiki/ClassLoadingConfiguration)
            -   Explains how-to setup class loading isolation of J2EE modules using <loader-repository> element in `jboss-app.xml`, `jboss.xml` or `jboss-web` file.
        -   [Configuring the Deployment Scanner in conf/jboss-service.xml](https://community.jboss.org/wiki/ConfiguringTheDeploymentScannerInConfjbossSystemxml)
        -   [JBoss deployment directory configuration](http://www.mastertheboss.com/jboss-configuration/jboss-deployment-directory-configuration)
        -   [Adding a custom deploy folder](https://access.redhat.com/site/documentation/en-US/JBoss_Enterprise_Web_Platform/5/html/Getting_Started_Guide/The_JBoss_Server___A_Quick_Tour-Hot-deployment_of_services_in_JBoss-Adding_a_custom_deploy_folder.html)
    -   **data access**
        -   [Config data-source](http://community.jboss.org/wiki/configdatasources)
        -   [Config data-source and monitor connection pools](http://www.mastertheboss.com/en/jboss-server/96-jboss-datasource-configuration.html)
    -   **security**
        -   [Secure the JMX Console](https://community.jboss.org/wiki/SecureTheJmxConsole)
        -   [Encrypt passwords of data-sources](http://community.jboss.org/wiki/encryptingdatasourcepasswords)
        -   [Config JCA login module](http://community.jboss.org/wiki/ConfigJCALoginModule)
    -   **networking**
        -   [Identify and configure TCP/UDP port used](http://community.jboss.org/wiki/UsingJBossBehindAFirewall)
    -   **programming**
        -   [EJB3 Tutorial for JBoss AS 4.x](http://docs.jboss.org/ejb3/docs/tutorial/)
        -   [EJB3 Reference for JBoss AS 4.x](http://docs.jboss.org/ejb3/docs/reference/build/reference/en/html/)
        -   [EJB3 Tutorial for JBoss AS 5.x](http://www.jboss.org/file-access/default/members/jbossejb3/freezone/docs/tutorial/1.0.7/html/index.html)
        -   [EJB3 Reference for JBoss AS 5.x](http://www.jboss.org/file-access/default/members/jbossejb3/freezone/docs/reference/1.0.7/html/index.html)

#### WildFly

-   References
    -   [Wildfly 10.0.0.Final Model Reference](http://wildscribe.github.io/Wildfly/10.0.0.Final/index.html)
    -   [WildFly 10 Documentation](https://docs.jboss.org/author/display/WFLY10/Documentation)
    -   [WildFly Quick Starts](https://github.com/wildfly/quickstart) : provide small, specific, working examples that can be used as a reference for your own project
    -   [WildFly 10 CLI Recipes](https://docs.jboss.org/author/display/WFLY10/CLI+Recipes)
    -   [Command line parameters](https://docs.jboss.org/author/display/WFLY10/Command+line+parameters)
        -   command-line parameters of `$JBOSS_HOME/bin/domain.sh` and `$JBOSS_HOME/bin/standalone.sh`

<!-- -->

-   Readings
    -   [WildFly 10 Deployment Scanner configuration](https://docs.jboss.org/author/display/WFLY10/Deployment+Scanner+configuration)
    -   [Useful JBoss and WildFly Command Line Parameters, System Properties and Environment Variables](http://developer-should-know.com/post/109693982502/useful-jboss-and-wildfly-command-line-parameters) (Jan 31st, 2015)
    -   [Adding WildFly User - `add-user.sh`](https://docs.jboss.org/author/display/WFLY10/add-user+utility)
    -   [Connecting VisualVM or JConsole to Wildfly 10](https://developer.jboss.org/thread/269919) (2016. 5. 20)
        -   [`service:jmx:http-remoting-jmx://hostname:9990`](service:jmx:http-remoting-jmx://hostname:9990)
    -   [Monitoring WildFly using VisualVM](http://www.mastertheboss.com/jboss-server/wildfly-8/monitoring-wildfly-using-visualvm) (04 July 2014)

#### Undertow

-   <http://undertow.io/>
-   Desc. : a flexible performant web server written in java, providing both blocking and non-blocking API’s based on NIO.
-   Written in : Java
-   License :

<!-- -->

-   Readings

#### TomEE

-   <http://openejb.apache.org/>
-   Desc : an all-Apache Java EE 6 Web Profile certified stack where Tomcat is top dog.

#### OpenEJB

-   <http://openejb.codehaus.org/>
-   Desc : an open source, modular, configurable, and extendable EJB Container System and EJB Server.
-   License :

### Application Server

#### Apache HTTP Server

-   References
    -   [Apache 2.4 Core Features](http://httpd.apache.org/docs/current/mod/core.html)
    -   Directives
        -   [**`Alias`** directive](http://httpd.apache.org/docs/2.2/mod/mod_alias.html#page-header)
        -   [**`Directory`** directive](http://httpd.apache.org/docs/current/mod/core.html#directory)
    -   [Apache Authentication via HTTP Basic or Digest](http://edoceo.com/howto/apache-authentication)
    -   [How can I check which version of apache is installed on a Debian machine?](http://stackoverflow.com/questions/289586/how-can-i-check-which-version-of-apache-is-installed-on-a-debian-machine) (Nov 14 '08)
    -   [How do I find the version of Apache running without access to the command line?](http://stackoverflow.com/questions/166607/how-do-i-find-the-version-of-apache-running-without-access-to-the-command-line) (Oct 3 '08)
    -   [Apache2: 'AH01630: client denied by server configuration'](http://stackoverflow.com/questions/18392741/apache2-ah01630-client-denied-by-server-configuration) (Aug 23 '13)
    -   [How To Configure Logging And Log Rotation In Apache On An Ubuntu VPS](https://www.digitalocean.com/community/tutorials/how-to-configure-logging-and-log-rotation-in-apache-on-an-ubuntu-vps) (August 19, 2013)

#### Nginx

-   <http://nginx.org/>
-   Desc. : a free, open-source, high-performance HTTP server and reverse proxy, as well as an IMAP/POP3 proxy server
-   License :
-   Written in : C
-   Readings
    -   [Nginx passes Apache as Web server of choice among top sites](http://www.infoworld.com/article/2608377/open-source-software/nginx-passes-apache-as-web-server-of-choice-among-top-sites.html)(May 27, 2014)
    -   [Nginx overtakes Microsoft as No. 2 Web server](http://www.infoworld.com/article/2618444/app-servers/nginx-overtakes-microsoft-as-no--2-web-server.html)(Jan 4, 2012)

#### mongoose

-   <https://github.com/cesanta/mongoose>
-   Desc. : Embedded web server for C/C++
-   License : GPL v2

#### ZooKeeper

-   <http://zookeeper.apache.org/>
-   Desc. : an open-source server which enables highly reliable distributed coordination.
-   License : Apache License
-   Written in : Java
-   Source
    -   master : <https://github.com/apache/zookeeper>
    -   release 3.3.6 : <https://github.com/apache/zookeeper/tree/release-3.3.6>
-   Maven artifacts : [`org.apache.zookeeper » zookeeper`](https://mvnrepository.com/artifact/org.apache.zookeeper/zookeeper)

<!-- -->

-   References
    -   [ZooKeeper 3.4 Documentation](http://zookeeper.apache.org/doc/r3.4.6/)
        -   [Administrator's Guide](https://zookeeper.apache.org/doc/r3.4.6/zookeeperAdmin.html)
            -   [Configuration Parameters](https://zookeeper.apache.org/doc/r3.4.6/zookeeperAdmin.html#sc_configuration)
            -   [Standalone Operation](https://zookeeper.apache.org/doc/r3.4.6/zookeeperStarted.html#sc_InstallingSingleMode)
            -   [Clustered Setup](https://zookeeper.apache.org/doc/r3.4.6/zookeeperAdmin.html#sc_zkMulitServerSetup)
    -   [ZooKeeper 3.4.6 API](http://zookeeper.apache.org/doc/r3.4.6/api/)
-   Readings
    -   [ZooKeeper JMX](http://zookeeper.apache.org/doc/r3.4.6/zookeeperJMX.html)
    -   [Running Zooinspector](https://github.com/sleberknight/zookeeper-samples/blob/master/etc/running-zooinspector.txt)
    -   [How to Run ZooInspector](http://braveo.blogspot.kr/2013/05/how-to-run-zooinspector.html)(31.5.13)
        -   Using Maven
    -   [Install and start Zookeeper server on Windows](https://support.lucidworks.com/hc/en-us/articles/203187153-Install-and-start-Zookeeper-server-on-Windows)(August 12, 2014)

### Media Streaming Server

#### LIVE555 Media Server

-   <http://www.live555.com/mediaServer/>
-   Desc. : <http://www.live555.com/mediaServer/>
-   License : LGPL
-   Sources :

#### Red5

-   <https://github.com/Red5>
-   Desc. : an Open Source Flash Server written in Java
-   License : Apache License 2.0
-   Sources : <https://github.com/Red5>
-   Maven artifacts : [org.red5](http://mavenrepository.com/artifact/org.red5)

<!-- -->

-   References
    -   [Red5 Server Wiki](https://github.com/Red5/red5-server/wiki)
    -   [Red5 Server 1.0 API](http://red5.googlecode.com/svn/doc/tags/1_0/api/index.html)
    -   [Red5 Client 1.0 API](http://red5.googlecode.com/svn/doc/tags/1_0/api-client/index.html)
    -   [Red5 - Reference Documentation](http://red5.googlecode.com/svn/doc/trunk/reference/html/index.html)
    -   [RTMP Specification](http://www.adobe.com/devnet/rtmp.html)
    -   [RTMP](https://en.wikipedia.org/wiki/Real_Time_Messaging_Protocol) (Wikipedia)
    -   [Flash Video Structure](https://en.wikipedia.org/wiki/Flash_Video#Flash_Video_Structure)
    -   [RTMP](http://wiki.multimedia.cx/?title=RTMP) (MultimediaWiki)
    -   [RTMP packet header and packet types](http://wiki.multimedia.cx/?title=RTMP#Packet_header)
        -   [`VideoCodec.java`](https://github.com/Red5/red5-io/blob/master/src/main/java/org/red5/codec/VideoCodec.java)

<!-- -->

-   Packages and Classes
    -   [`Constants`](http://red5.googlecode.com/svn/doc/tags/1_0/api/index.html?org/red5/server/net/rtmp/message/Constants.html)
        -   includes constants for RTMP packet types and shared objects
    -   [`RTMPClient extends BaseRTMPClientHandler`](https://github.com/Red5/red5-client/blob/master/src/main/java/org/red5/client/net/rtmp/RTMPClient.java)
    -   [`BaseRTMPClientHandler extends BaseRTMPHandler implements IRTMPClient`](https://github.com/Red5/red5-client/blob/master/src/main/java/org/red5/client/net/rtmp/BaseRTMPClientHandler.java)
    -   [`BaseRTMPHandler implements IRTMPHandler, Constants, StatusCodes`](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/BaseRTMPHandler.java)
        -   [`BaseRTMPHandler.messageReceived(RTMPConnection conn, Packet packet)`](https://github.com/Red5/red5-server-common/blob/v1.0.5-RELEASE/src/main/java/org/red5/server/net/rtmp/BaseRTMPHandler.java#L65)
    -   [`RTMPConnection`](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/RTMPConnection.java)
        -   [`public void invoke(IServiceCall call, int channel)` method](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/RTMPConnection.java#L1067)
        -   [`public Channel getChannel(int channelId)` method](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/RTMPConnection.java#L551)
        -   [`public OutputStream createOutputStream(int streamId)` method](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/RTMPConnection.java#L663)
        -   [`public void handleMessageReceived(Packet message)` method](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/RTMPConnection.java#L1258)
            -   The place where to capture all the packets
    -   [`Channel`](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/Channel.java)
        -   [`public void write(IRTMPEvent event)` method](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/Channel.java#L98)
    -   [`org.red5.server.jmx.mxbeans` packages](http://red5.googlecode.com/svn/doc/tags/1_0/api/index.html?org/red5/server/jmx/mxbeans/package-summary.html)
    -   **Events**
        -   [`Aggreate.java` source](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/net/rtmp/event/Aggregate.java)
            -   Aggreation of video data and audio data
    -   **Built-in Streams**
        -   [`AbstractClientStream`](http://red5.googlecode.com/svn/doc/tags/1_0/api/index.html?org/red5/server/stream/AbstractClientStream.html)
        -   [**`ClientBroadcastStream.java`**](https://github.com/Red5/red5-server-common/blob/master/src/main/java/org/red5/server/stream/ClientBroadcastStream.java)
            -   Mandatory sample for real-world stream implementation
            -   Note on `ClientBroadcastStreamMXBean`, `getCodecInfo()`, `metaData` and call of `VideoCodecFactory.getVideoCodec`.

<!-- -->

-   Readings
    -   [How to format Adobe Flash RTMP URLs](http://www.wowza.com/forums/content.php?55-How-to-format-Adobe-Flash-RTMP-URLs)(10-31-2014)
    -   [**How to record RTMP flash video streams using Red5**](https://ptrthomas.wordpress.com/2008/04/19/how-to-record-rtmp-flash-video-streams-using-red5/)(2009-04-04)
    -   [How to extract snapsot image from RTMP stream’s videodata in Red5 App](http://www.mekya.com/2014/11/07/how-to-extract-snapsot-image-from-rtmp-streams-videodata-in-red5-app/)
    -   [Using RTMP Streaming](http://support.jwplayer.com/customer/portal/articles/1430358-using-rtmp-streaming)
    -   [RTMP protocol and extract RTMP video streams H264 video files](http://www.programmershare.com/3636628/)(2014-02-02)
    -   [Writing a Red5 Java Web App to Handle RTMP Streams](https://www.peterbeard.co/blog/post/writing-a-red5-java-web-app-to-handle-rtmp-streams/)(2012-12-04)
    -   [Extract frames as images from an RTMP stream in real-time](http://stackoverflow.com/questions/21514392/extract-frames-as-images-from-an-rtmp-stream-in-real-time)(Feb 2 '14)

#### Darwin Streaming Server

-   <http://dss.macosforge.org/>
-   Desc. : the open source server technology that allows you to send streaming media to clients across the Internet using the industry standard RTP and RTSP protocols.
-   License : Apple Public Source License

### NoSQL

-   [Classification of NoSQL](http://en.wikipedia.org/wiki/NoSQL#Classification_based_on_data_model)
-   [Distributed cache](https://en.wikipedia.org/wiki/Distributed_cache)
-   [Java Caches: Ehcache, Hazelcast, Infinispan](https://labs.consol.de/java-caches/index.html)
-   [A comparative study of distributed caches](http://blog.engineering.aol.com/2015/08/28/a-comparative-study-of-distributed-caches/) (August 28, 2015)
    -   Memcached, Redis, Hazelcast
-   [Comparing Distributed Caching Frameworks](http://blog.tarams.com/index.php/2015/comparing-distributed-caching-frameworks/) (Apr 6, 2015)
    -   Hazelcast, Memcached, Redis
-   [**From distributed caches to in-memory data grids**](http://www.slideshare.net/MaxAlexejev/from-distributed-caches-to-inmemory-data-grids) (May 20, 2012)

#### Redis

-   <http://redis.io/>
-   Desc : an open-source, networked, in-memory, key-value data store with optional durability.
-   License : BSD
-   Readings
    -   [Commands](http://redis.io/commands)
    -   [An introduction to Redis data types and abstractions](http://redis.io/topics/data-types-intro)
    -   [Data types](http://redis.io/topics/data-types)
    -   [Redis configuration](http://redis.io/topics/config)
    -   [The Little Redis Book](https://github.com/karlseguin/the-little-redis-book)
    -   [Example of storing nested data structures in redis](https://gist.github.com/petrushev/6192270)
    -   [How to access Redis log file](http://stackoverflow.com/questions/16337107/how-to-access-redis-log-file) (May 2 '13)
    -   [Redis-server - FATAL CONFIG FILE ERROR - Filepath Space problem](http://stackoverflow.com/questions/6045947/redis-server-fatal-config-file-error-filepath-space-problem)(May 18 '11)
        -   Use “`C:/Program Files (x86)/My Application/log/redis.log`” instead of “`C:\Program Files (x86)\My Application\log\redis.log`”

##### lettuce

-   <https://github.com/mp911de/lettuce>
-   Desc. : Advanced Redis client for thread-safe sync, async, and reactive usage.
-   License : Apache License 2.0
-   Readings
    -   [lettuce 4.1.2.Final AP](http://redis.paluch.biz/docs/api/releases/4.1.2.Final/)
    -   [lettuce Wiki](https://github.com/mp911de/lettuce/wiki)

##### Spring Data Redis

-   <http://projects.spring.io/spring-data-redis/>
-   Desc. : provides easy configuration and access to Redis from Spring applications.

##### embedded-redis

-   <https://github.com/kstyrc/embedded-redis>
-   Desc. : Redis embedded server for Java integration testing
-   Licesne : Apache License, Version 2.0
-   Readings

##### FastoRedis

-   <http://fastoredis.com/>
-   Desc. : a cross-platform open source Redis management tool (i.e. Admin GUI)
-   License : GPL
-   Readings

#### MongoDB

-   <https://www.mongodb.org/>
-   Desc. : an open-source, document database designed for ease of development and scaling
-   License : Apache License Version 2.0
-   Sources : <https://github.com/mongodb/mongo>

<!-- -->

-   Readings
    -   [Configure a Windows Service for MongoDB](http://docs.mongodb.org/master/tutorial/install-mongodb-on-windows/#configure-a-windows-service-for-mongodb)
    -   [Configuration File Options](http://docs.mongodb.org/master/reference/configuration-options/)
    -   [`mongod.exe`](http://docs.mongodb.org/master/reference/program/mongod.exe/)

#### CouchDB

-   <http://couchdb.apache.org/>
-   Desc. : open source database software that focuses on ease of use and having an architecture that “completely embraces the Web”
-   License :
-   Sources :

<!-- -->

-   References
    -   [Apache CouchDB 2.0 Documentation](http://docs.couchdb.org/en/2.0.0/)
        -   [Configuration](http://docs.couchdb.org/en/2.0.0/config/)
        -   [Configuration Reference](http://docs.couchdb.org/en/2.0.0/config-ref.html)
        -   [Performance](http://docs.couchdb.org/en/2.0.0/maintenance/performance.html)

<!-- -->

-   Readings
    -   [Erlang `inet` module &gt; socket options](http://erlang.org/doc/man/inet.html#setopts-2)
        -   `httpd` / `socket_options`
    -   [MochiWeb startup options](https://github.com/mochi/mochiweb/blob/v2.16.0/src/mochiweb_http.erl#L56)
        -   `httpd` / `server_options`
        -   `name, ip, backlog, nodelay, acceptor_pool_size, ssl, profile_fun, link, recbuf`
    -   [CouchDB JIRA &gt; Keep Alive connections with zero size attachments](https://issues.apache.org/jira/browse/COUCHDB-3394)

<!-- -->

-   Books
    -   [*CouchDB The Definitive Guide*](http://guide.couchdb.org/index.html)

### Version Control Software

-   [Comparison of version control software](https://en.wikipedia.org/wiki/Comparison_of_version_control_software)

#### Subversion

-   Download
    -   [Apache Subversion Binary Packages](http://subversion.apache.org/packages.html)

<!-- -->

-   References
    -   [Version Control with Subversion 1.8 (draft)](http://svnbook.red-bean.com/nightly/en/)
    -   [Version Control with Subversion 1.7](http://svnbook.red-bean.com/en/1.7/)
        -   [Compatibility concerns](http://subversion.apache.org/docs/release-notes/1.7.html#compatibility)
            -   Older clients and servers interoperate transparently with 1.7 servers and clients.
            -   Existing working copies created with Subversion 1.6 and earlier need to be upgraded before they can be used with a Subversion 1.7 client.
        -   [Working Copy Metadata Storage Improvements](http://subversion.apache.org/docs/release-notes/1.7.html#wc-ng)
        -   [Traversing Branches](http://svnbook.red-bean.com/en/1.7/svn.branchmerge.switchwc.html)
    -   [Version Control with Subversion 1.6](http://svnbook.red-bean.com/en/1.6/)
    -   [Version Control with Subversion 1.5](http://svnbook.red-bean.com/en/1.5/)
    -   [Version Control with Subversion 1.4](http://svnbook.red-bean.com/en/1.4/)
    -   [Subversion runtime configuration](http://svnbook.red-bean.com/en/1.4/svn.advanced.confarea.html)

<!-- -->

-   Readings
    -   [Using negative patterns for Subversion's <svn:ignore> property](http://www.thoughtspark.org/node/38)
    -   [Set <svn:ignore> for multiple files from command line](http://kthoms.wordpress.com/2011/04/21/set-svnignore-for-multiple-files-from-command-line/)
    -   [Migrating Subversion Repository Data Elsewhere](http://svnbook.red-bean.com/en/1.4/svn.reposadmin.maint.html#svn.reposadmin.maint.migrate)
    -   [Integrating Redmine with Subversion](http://andrew.lansdowne.me/2012/10/01/integrating-redmine-with-subversion/)(October 1, 2012)
    -   [Best way to link Redmine issue to SVN revision](http://stackoverflow.com/questions/3607590/best-way-to-link-redmine-issue-to-svn-revision)
    -   client configuration of Subvesion
        -   The location of client configuration files for subversion is dependent on operating system.
            -   Windows : `%USERPROFILE%\Application Data\Subversion\config`
            -   Linux : `~/.subversion/config`
        -   [Configurable options](http://svnbook.red-bean.com/en/1.5/svn.advanced.confarea.html#svn.advanced.confarea.opts.config)
    -   [Per repository AuthzSVNAccessFile apache rule with a single apache config file or other shell script solution](http://stackoverflow.com/questions/20980262/per-repository-authzsvnaccessfile-apache-rule-with-a-single-apache-config-file-o) (Jan 7 2014)
    -   [Add all unversioned files to SVN](http://stackoverflow.com/questions/1598968/add-all-unversioned-files-to-svn)(Oct 21 '09)
    -   [**What is the correct way to restore a deleted file from SVN?**](http://stackoverflow.com/questions/490522/what-is-the-correct-way-to-restore-a-deleted-file-from-svn)(Jan 29 '09)
    -   [How do I tell Subversion to treat a file as a binary file?](http://stackoverflow.com/questions/73797/how-do-i-tell-subversion-to-treat-a-file-as-a-binary-file)(Sep 16 '08)
        -   [`svn:mime-type`](svn:mime-type)` = application/octet-stream`
    -   [How does Subversion handle binary files?](http://help.collab.net/index.jsp?topic=/faq/svnbinary.html)
    -   **`svn checkout`**
        -   [Shallow checkouts](http://svnbook.red-bean.com/en/1.6/svn.advanced.sparsedirs.html)
    -   **`svn diff`**
        -   [Unified diff format](http://en.wikipedia.org/wiki/Diff_utility#Unified_format)
        -   [how to make svn diff show only non-whitespace line changes between two revisions](http://stackoverflow.com/questions/1741705/how-to-make-svn-diff-show-only-non-whitespace-line-changes-between-two-revisions)(Nov 16 '09)
    -   **`svn update`**
        -   [How do I avoid “svn: Out of Date:” problems?](http://stackoverflow.com/questions/815604/how-do-i-avoid-svn-out-of-date-problems) (May 2 '09)
        -   [I'm trying to commit, but Subversion says my working copy is **out of date** ?](http://subversion.apache.org/faq.html#wc-out-of-date)
        -   [Is it possible to always (force) overwrite local changes when updating from SVN? Ignore conflicts?](http://stackoverflow.com/questions/3709197/is-it-possible-to-always-force-overwrite-local-changes-when-updating-from-svn)(Sep 14 '10)
            -   update ignoring(overwriting) local change = `svn revert` + `svn update`
    -   **`svn delete`**
        -   [How do you remove Subversion control for a folder?](http://stackoverflow.com/questions/154853/how-do-you-remove-subversion-control-for-a-folder)(Sep 30 '08)
            -   Remarks on '`svn delete --keep-local`' and single `.svn` directory per working copy feature of Subversion 1.7.

<!-- -->

-   Examples
    -   `$ svn info . //show status including working copy path, repository path, base revision and et al.`
    -   `$ svn status . //show only locally modified items under the current directory of working copy`
    -   `$ svn update //update working copy`
    -   `$ svn commit //send changes from your working copy to the repository`
    -   `$ svn --force --depth infinity add . //add all unversioned files to local change`
    -   `$ svn copy ^/trunk/foundation ^/branches/foundation-20141123 -c `“`Created`` ``a`` ``new`` ``branch`` ``of`` ``/trunk/foundation`”` //making branch`

#### Git

-   References
    -   [Git Documentation](http://git-scm.com/documentation)
    -   [Git Reference](http://git-scm.com/docs)
        -   [`git-clone`](http://git-scm.com/docs/git-clone)
        -   [`git-status`](http://git-scm.com/docs/git-status)
        -   [`git-add`](http://git-scm.com/docs/git-add)
        -   [`git-commit`](http://git-scm.com/docs/git-commit)
        -   [`git-push`](http://git-scm.com/docs/git-push)
        -   [`git-pull`](http://git-scm.com/docs/git-pull)
        -   [`git-branch`](http://git-scm.com/docs/git-branch)
        -   [`git-checkout`](http://git-scm.com/docs/git-checkout)
        -   [`gitignore`](http://git-scm.com/docs/gitignore) (`$HOME/.config/git/ignore, $GIT_DIR/info/exclude, .gitignore`)
        -   [`git-diff`](https://git-scm.com/docs/git-diff)
        -   [`git-rebase`](https://git-scm.com/docs/git-rebase)
        -   [`git-reset --hard`](https://git-scm.com/docs/git-reset#git-reset---hard)
    -   [*Pro Git*](https://git-scm.com/book/en/v2)
        -   [**Git References**](https://git-scm.com/book/en/v2/Git-Internals-Git-References)
        -   [Git Branching](https://git-scm.com/book/en/v2/Git-Branching-Branches-in-a-Nutshell)
    -   [GitHub Bootcamp](https://help.github.com/categories/54/articles)
        -   As of Nov. 1st 2012, one important thing is missing. That is sharing the project in Eclipse, after cloning the fork in the server. Maybe it could be a defect of Eclipse, not a miss of the article.
    -   [Git Cheatsheet](http://ndpsoftware.com/git-cheatsheet.html)
        -   \[<http://ndpsoftware.com/git-cheatsheet.html#loc=workspace>; Workspace\]
        -   \[<http://ndpsoftware.com/git-cheatsheet.html#loc=local_repo>; Local Repository\]
        -   \[<http://ndpsoftware.com/git-cheatsheet.html#loc=remote_repo>; Upstream Repository\]
    -   [Git Tutorials](https://www.atlassian.com/git/tutorial)
    -   [Git Workflows](https://www.atlassian.com/git/workflows)
    -   [Git Refs](http://wiki.eclipse.org/EGit/User_Guide#Git_References)

<!-- -->

-   Articles
    -   [What's a “fast-forward” in Git?](http://stackoverflow.com/questions/4684352/whats-a-fast-forward-in-git)
    -   [Is there any way to clone a git repository's sub-directory only?](http://stackoverflow.com/questions/600079/is-there-any-way-to-clone-a-git-repositorys-sub-directory-only)
    -   [Removing a remote](https://help.github.com/articles/removing-a-remote)
    -   [How to change file premissions in Git on Windows](http://blog.lesc.se/2011/11/how-to-change-file-premissions-in-git.html) (Nov. 2011)
    -   [Git - Get Current Working Copy Version](http://stackoverflow.com/questions/8974608/git-get-current-working-copy-version) (Jan 23 '12)
        -   `git log -1`
    -   [How to get latest git commit hash command](http://stackoverflow.com/questions/15677439/how-to-get-latest-git-commit-hash-command) (Mar 28 '13)
    -   [Dealing with line endings](https://help.github.com/articles/dealing-with-line-endings/)
    -   [What does tree-ish mean in Git?](http://stackoverflow.com/questions/4044368/what-does-tree-ish-mean-in-git/)(Oct 28 '10)
    -   [Ignoring files](https://help.github.com/articles/ignoring-files/)
    -   [gitignore: Ignore all files in folder hierarchy except one specific filetype](https://stackoverflow.com/questions/7803689/gitignore-ignore-all-files-in-folder-hierarchy-except-one-specific-filetype)(
    -   [Git v2.0.0, what changed, and why should you care](https://felipec.wordpress.com/2014/05/29/git-v2-0-0/) (2014/05/29)
    -   [**Adding an existing project to GitHub using the command line**](https://help.github.com/articles/adding-an-existing-project-to-github-using-the-command-line/)
    -   **Revision**
        -   [What is a Git Revision Expression?](https://stackoverflow.com/questions/1215814/what-is-a-git-revision-expression) (Aug 1 '09)
        -   [Revision Selection](https://git-scm.com/book/en/v2/Git-Tools-Revision-Selection)
        -   [gitrevisions](https://git-scm.com/docs/gitrevisions)
        -   [What is the parent of a git commit? How can there be more than one parent to a git commit?](https://stackoverflow.com/questions/38239521/what-is-the-parent-of-a-git-commit-how-can-there-be-more-than-one-parent-to-a-g) (Jul 7 '16)
    -   **Checkout**
        -   [How to checkout remote git tag](https://stackoverflow.com/questions/35979642/how-to-checkout-remote-git-tag) (Mar 14 '16)
            -   `git checkout v1.0.0, git checkout tags/v1.0.0`
        -   [How to get back to most recent version in Git?](https://stackoverflow.com/questions/3559076/how-to-get-back-to-most-recent-version-in-git) (Aug 24 '10)
            -   `git checkout master`
    -   **Subtree**
        -   [**About Git subtree merges**](https://help.github.com/articles/about-git-subtree-merges/)
            -   to manage multiple projects within a single repository
        -   [Mastering Git subtrees](https://medium.com/@porteneuve/mastering-git-subtrees-943d29a798ec)
    -   **Diff**
        -   [How to diff the same file between two different commits on the same branch?](https://stackoverflow.com/questions/3338126/how-to-diff-the-same-file-between-two-different-commits-on-the-same-branch) (Jul 26 '10)
        -   [Git diff between given two tags](https://stackoverflow.com/questions/3211809/git-diff-between-given-two-tags) (Jul 9 '10)

<!-- -->

-   Tools
    -   [Interfaces, frontends, and tools](https://git.wiki.kernel.org/index.php/InterfacesFrontendsAndTools)

<!-- -->

-   Examples

``` bash
$ git pull origin
$ git status .   //identifies added, modified or untracked items under current local directory
$ git add .      //updates index
$ git status .   //confirms the changes to be committed
$ git commit -m "message ..."   //commits changes into local repository
$ git status .   //confirms all changes are committed
$ git push       //pushes changes in local repository onto the remote repository
```

``` bash
$ git branch    //lists branches
$ git tag       //lists tags
$ git checkout release-1.1   //updates working tree to release-1.1
```

``` bash
$ git reset --hard  //Resets the index and working tree. Any changes to tracked files in the working tree since <commit> are discarded.
```

#### GitHub

-   [GitHub Help](https://help.github.com/)
-   [GitHub Pages](https://pages.github.com/)
-   [**GitHub Pages Basics**](https://help.github.com/categories/github-pages-basics/) `gh-pages`
-   [Creating Project Pages manually](https://help.github.com/articles/creating-project-pages-manually/)
-   [User, Organization, and Project Pages](https://help.github.com/articles/user-organization-and-project-pages/)
-   [GitHub Flavored Markdown](https://help.github.com/articles/github-flavored-markdown/)
-   [Searching code](https://help.github.com/articles/searching-code/)

<!-- -->

-   [Get Started With GitHub Pages (Plus Bonus Jekyll)](https://24ways.org/2013/get-started-with-github-pages/)(18 December 2013)

#### EGit

-   <http://www.eclipse.org/egit/>
-   Desc : an Eclipse Team provider for the Git version control system.
-   License
-   Readings
    -   [EGit User Guide](http://wiki.eclipse.org/EGit/User_Guide)
        -   [Resolving a merge conflict](http://wiki.eclipse.org/EGit/User_Guide#Resolving_a_merge_conflict)
    -   [Git For Eclipse Users](http://wiki.eclipse.org/EGit/Git_For_Eclipse_Users)
    -   [EGit Getting Started](http://wiki.eclipse.org/EGit/User_Guide/Getting_Started)
    -   [EGit/GitHub/User Guide](http://wiki.eclipse.org/EGit/GitHub/UserGuide) : explains how to use EGit Mylyn GitHub connector.
    -   [EGit Compare shows all lines as changed](http://stackoverflow.com/questions/20844198/egit-compare-shows-all-lines-as-changed) (Dec 30 '13)
        -   EGit doesn't support `.gitattributes` file.

#### Harvest

-   Readings
    -   [CA SCM (Harvest) 12.1 Documentation](ftp://mf.cai.com/caproducts/cccharvest/r121/Beta/Bookshelf/Bookshelf.html)
    -   [.scmignore file of Harvest](ftp://mf.cai.com/caproducts/cccharvest/r121/Beta/Bookshelf/Bookshelf_Files/HTML/CA_SCM_Workbench_ENU/index.htm?toc.htm?ignore_files.html)

#### Gitorious

-   Desc : Git hosting and collaboration software that you can install yourself
-   License : GNU Affero General Public License

#### GitLab

-   <https://github.com/gitlabhq/gitlabhq>
-   Desc : self hosted Git management software
-   License : MIT license

#### Team Foundation Version Control

-   <https://msdn.microsoft.com/en-us/library/ms181237.aspx>
-   Desc. : a centralized version control system.

<!-- -->

-   Readings
    -   [Customize which files are ignored by version control](https://msdn.microsoft.com/en-us/library/ms245454.aspx#tfignore)
        -   `.tfignore`, recursive by default

### Repository Management Software

#### Artifactory

-   <http://sourceforge.net/projects/artifactory/>
-   Desc : Binary Repository Manager for Maven, Ivy, Gradle modules, etc.
-   License : LGPLv3

### Configuration Management Software

-   [Comparison of open-source configuration management software](https://en.wikipedia.org/wiki/Comparison_of_open-source_configuration_management_software)
-   [Deployment Management Tools: Chef vs. Puppet vs. Ansible vs. SaltStack vs. Fabric](http://blog.takipi.com/deployment-management-tools-chef-vs-puppet-vs-ansible-vs-saltstack-vs-fabric/) (August 5, 2015)

#### Chef

-   <https://www.chef.io/>
-   Desc. : a powerful automation platform that transforms infrastructure into code
-   License : Apache License 2.0

<!-- -->

-   References
    -   [Resources Reference](https://docs.chef.io/resources.html)

#### Puppet

-   <https://puppet.com/>
-   Desc. :
-   License :

<!-- -->

-   References
    -   [Official documentations](https://docs.puppet.com/puppet/)
    -   **4.10**
        -   [Resource Type Reference](https://docs.puppet.com/puppet/4.10/type.html)

#### Ansible

-   <https://www.ansible.com/>
-   Desc. : can configure systems, deploy software, and orchestrate more advanced IT tasks such as continuous deployments or zero downtime rolling updates.
-   License :
-   Written in :
-   Sources : <https://github.com/ansible/ansible>

<!-- -->

-   References
    -   [Ansible Documentation](http://docs.ansible.com/ansible/)

#### Fabric

-   <http://www.fabfile.org/>
-   Desc. : a Python library and command-line tool for streamlining the use of SSH for application deployment or systems administration tasks.
-   License :
-   Written in : Python
-   Sources : <https://github.com/fabric/fabric>

<!-- -->

-   References
    -   **1.13**
        -   [Official documentation](http://docs.fabfile.org/en/1.13/)
        -   [Overview and Tutorial](http://docs.fabfile.org/en/1.13/tutorial.html)

#### SaltStack

-   <https://saltstack.com/>
-   Desc. : Software to automate the management and configuration of any infrastructure or application at scale.
-   Sources : <https://github.com/saltstack/salt>

<!-- -->

-   Readings
    -   [SaltStack Documentation](https://docs.saltstack.com/en/latest/)

### System Monitoring Software

#### Ganglia

-   <http://ganglia.info/>
-   Desc : a scalable distributed monitoring system for high-performance computing systems such as clusters and grids.
-   License : BSD
-   Written in : C, Perl, PHP, Python
-   Source : <https://github.com/ganglia>
-   Readings
    -   [Wiki for Ganglia](http://sourceforge.net/apps/trac/ganglia)
    -   [Ganglia on Wikipedia](http://en.wikipedia.org/wiki/Ganglia_(software))

#### Graphite

-   <https://github.com/graphite-project>
-   Desc. : an enterprise-scale monitoring tool that runs well on cheap hardware.
-   License : The Apache 2.0 License
-   Written in : Python
-   Sources : <https://github.com/graphite-project>
-   Readings
    -   [Official Documentation](http://graphite.readthedocs.org/en/latest/)

#### Prometheus

-   <https://prometheus.io/>
-   Desc. : an open-source systems monitoring and alerting toolkit originally built at SoundCloud.
-   License :
-   Sources
    -   <https://github.com/prometheus/prometheus>
    -   <https://github.com/prometheus/node_exporter>
-   Distributions
    -   Ubuntu : <http://packages.ubuntu.com/xenial/prometheus-node-exporter>
    -   Debian : <https://packages.debian.org/sid/net/prometheus>
-   Docker Images
    -   <https://hub.docker.com/r/prom/prometheus/>
    -   <https://hub.docker.com/r/prom/node-exporter/>

<!-- -->

-   References
    -   [Configuration](https://prometheus.io/docs/operating/configuration/)
        -   [<scrape_config>](https://prometheus.io/docs/operating/configuration/#%3Cscrape_config%3E) : specifies a set of targets and parameters describing how to scrape them
        -   [<file_sd_config>](https://prometheus.io/docs/operating/configuration/#%3Cfile_sd_config%3E) : provides a more generic way to configure static targets and serves as an interface to plug in custom service discovery mechanisms.
        -   [<relabel_config>](https://prometheus.io/docs/operating/configuration/#%3Crelabel_config%3E) : a powerful tool to dynamically rewrite the label set of a target before it gets scraped.
    -   [Querying](https://prometheus.io/docs/querying/basics/)
    -   [Functions](https://prometheus.io/docs/querying/functions/)
    -   [Exporters](https://github.com/prometheus/docs/blob/master/content/docs/instrumenting/exporters.md)

<!-- -->

-   Readings
    -   [Installing Prometheus Using Docker](https://prometheus.io/docs/introduction/install/#using-docker)
    -   [How To Use Prometheus to Monitor Your Ubuntu 14.04 Server](https://www.digitalocean.com/community/tutorials/how-to-use-prometheus-to-monitor-your-ubuntu-14-04-server) (June 30, 2015)
    -   [How To Set Up Prometheus To Monitor Your Ubuntu Server](https://blog.100tb.com/how-to-set-up-prometheus-to-monitor-your-ubuntu-server) (Mar 15, 2016)
    -   [**How To Install Prometheus using Docker on Ubuntu 14.04**](https://www.digitalocean.com/community/tutorials/how-to-install-prometheus-using-docker-on-ubuntu-14-04)(January 12, 2016)
    -   [Monitoring linux stats with Prometheus.io](https://resin.io/blog/monitoring-linux-stats-with-prometheus-io/)
    -   [SUSE System Monitoring Utilities](https://www.suse.com/documentation/sles11/book_sle_tuning/data/cha_util.html)
    -   **Label**
        -   [Automatically generated labels and time series](https://prometheus.io/docs/concepts/jobs_instances/#automatically-generated-labels-and-time-series) : `job`, `instance`
        -   [Life of a Label](https://www.robustperception.io/life-of-a-label/)(March 9, 2016)
        -   [Controlling the instance label](https://www.robustperception.io/controlling-the-instance-label/)(August 15, 2016)

<!-- -->

-   Collectors and Metrics
    -   [Understanding Machine CPU usage](https://www.robustperception.io/understanding-machine-cpu-usage/)
    -   [Exploring the `/proc/net/` Directory](http://www.onlamp.com/pub/a/linux/2000/11/16/LinuxAdmin.html)

<table>
<thead>
<tr class="header">
<th><p>Collector</p></th>
<th><p>Metrics</p></th>
<th><p>References</p></th>
<th><p>Remarks</p></th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td><p><code>meminfo</code></p></td>
<td><p><code>node_memory_MemTotal</code><br />
<code>node_memory_MemFree</code><br />
<code>node_memory_Buffers</code><br />
<code>node_memory_Cached</code></p></td>
<td><p><a href="https://www.centos.org/docs/5/html/5.1/Deployment_Guide/s2-proc-meminfo.html">CentOS &gt; <code>/proc/meminfo</code></a><br />
<a href="https://www.suse.com/documentation/sles11/book_sle_tuning/data/sec_util_memory.html">SUSE &gt; System Monitoring &gt; Memeory</a></p></td>
<td><p><code>/proc/meminfo</code></p></td>
</tr>
<tr class="even">
<td><p><code>netdev</code></p></td>
<td><p><code>node_network_receive_bytes</code><br />
<code>node_network_transmit_bytes</code></p></td>
<td></td>
<td><p><code>/proc/net/dev</code></p></td>
</tr>
</tbody>
</table>

-   Metrics and Labels

| Metric                       | Labels        | Remarks |
|------------------------------|---------------|---------|
| `node_cpu`                   | `cpu`, `mode` |         |
| `node_disk_io+*`             | `device`      |         |
| `node_network_receive_bytes` | `device`      |         |

-   Typical Queries

| Measure                                                                   | Query                                                                                                                                                                                                                                 | Remarks |
|---------------------------------------------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|---------|
| Non-idle CPU usage of the specified physical node                         | `100 - avg(irate(node_cpu{job="node",instance="$instance",mode="idle"}[2m])) * 100`                                                                                                                                                   |         |
| By-mode CPU usage of the specified physical node                          | `(avg (irate(node_cpu{job="node",instance="$instance"}[2m])) by (mode)) * 100`                                                                                                                                                        |         |
| Used memory(RAM) amount of the specified physical node                    | `(node_memory_MemTotal{instance="$instance"} - (node_memory_MemFree{instance="$instance"} + node_memory_Buffers{instance="$instance"} + node_memory_Cached{instance="$instance"}))/ node_memory_MemTotal{instance="$instance"} * 100` |         |
| Network sent traffic rate by interface of the specified physical node     | `rate(node_network_transmit_bytes{job="node",instance="$instance",device=~"lo|^eth.*"}[2m])`                                                                                                                                          |         |
| Network received traffic rate by interface of the specified physical node | `rate(node_network_receive_bytes{job="node",instance="$instance",device=~"lo|^eth.*"}[2m])`                                                                                                                                           |         |

##### Tips and Tricks

###### Available configuration flags of Node Exporter

“`/usr/bin/prometheus-node-exporter -h`” yields the following.

``` bash
  -auth.pass string
        Password for basic auth.
  -auth.user string
        Username for basic auth.
  -collector.diskstats.ignored-devices string
        Regexp of devices to ignore for diskstats. (default "^(ram|loop|fd|(h|s|v|xv)d[a-z])\\d+$")
  -collector.filesystem.ignored-mount-points string
        Regexp of mount points to ignore for filesystem collector. (default "^/(sys|proc|dev)($|/)")
  -collector.ipvs.procfs string
        procfs mountpoint. (default "/proc")
  -collector.megacli.command string
        Command to run megacli. (default "megacli")
  -collector.netdev.ignored-devices string
        Regexp of net devices to ignore for netdev collector. (default "^$")
  -collector.ntp.server string
        NTP server to use for ntp collector.
  -collector.textfile.directory string
        Directory to read text files with metrics from.
  -collectors.enabled string
        Comma-separated list of collectors to use. (default "diskstats,filesystem,loadavg,meminfo,stat,textfile,time,netdev,netstat")
  -collectors.print
        If true, print available collectors and exit.
  -debug.memprofile-file string
        Write memory profile to this file upon receipt of SIGUSR1.
  -log.level value
        Only log messages with the given severity or above. Valid levels: [debug, info, warn, error, fatal, panic]. (default info)
  -web.listen-address string
        Address on which to expose metrics and web interface. (default ":9100")
  -web.telemetry-path string
        Path under which to expose metrics. (default "/metrics")
```

###### Overriding `instance` label

Candidate 1 : Using `relabel_configs`

``` yaml
  - scrape_config:
    job_name: node
    static_configs:
      - targets: ['10.0.0.1:9100/brick1' '10.0.02:9100/brick1']

    relabel_configs:
      - source_labels: [__address__]
        target_label: instance
        regex: ^[^/]*/(.*)
        replacement: $1
      - source_labels: [__address__]
        target_label: __address__
        regex: ^([^/]*).*
        replacement: $1
```

Candidate 2 : Using `lables` for each target

``` yaml
  - scrape_config:
    job_name: node
    static_configs:
      - targets: ['10.0.0.1:9100']
        lables:
          "instance":"brick1"
          "role":"peer"
      - targets: ['10.0.0.1:9100']
        lables:
          "instance":"brick1"
          "role":"peer"
```

##### Companions

###### WMI Exporter

-   <https://github.com/martinlindhe/wmi_exporter>
-   Desc. : Prometheus exporter for Windows machines, using the WMI (Windows Management Instrumentation).
-   License : The MIT License (MIT)

#### Grafana

-   <https://grafana.com/>
-   Desc. : an open source metric analytics & visualization suite.
-   License
-   Sources : <https://github.com/grafana/grafana>
-   Docker Image
    -   <https://github.com/grafana/grafana-docker>
    -   <https://hub.docker.com/r/grafana/grafana/>

<!-- -->

-   References
    -   [Official documentation](http://docs.grafana.org/)
    -   [Configuration](http://docs.grafana.org/installation/configuration/)
    -   [HTTP API Reference](http://docs.grafana.org/http_api/)
    -   [Time Range Controls](http://docs.grafana.org/reference/timerange/)
    -   [Dashboard Export & Import](http://docs.grafana.org/reference/export_import/)
    -   [Dashboard Templating](http://docs.grafana.org/reference/templating/)
        -   [Prometheus templating queries](http://docs.grafana.org/features/datasources/prometheus/#templating)

<!-- -->

-   Readings
    -   [Using InfluxDB in Grafana](http://docs.grafana.org/features/datasources/influxdb/)
    -   [Grafana support for Prometheus](https://prometheus.io/docs/visualization/grafana/)

### Wiki

#### DokuWiki

-   <https://www.dokuwiki.org/>
-   Desc. : a simple to use and highly versatile Open Source wiki software that doesn't require a database.
-   License : GPL
-   Written in : PHP

#### MoinMoin

-   <http://moinmo.in/>
-   Desc : an advanced, easy to use and extensible WikiEngine with a large community of users.
-   License : GPL

#### gollum

-   <https://github.com/gollum/gollum>
-   Desc : a simple wiki system built on top of Git that powers GitHub Wikis.
-   License : The MIT License
-   Written in : Ruby

### Contents Management System

#### Drupal

-   <http://drupal.org/>
-   Desc : a powerful content management system which allows you to create and maintain many different types of websites without needing to know any coding languages.
-   License : GPL

#### Joomla

-   <http://www.joomla.org/>
-   Desc : a content management system (CMS), which enables you to build Web sites and powerful online applications.
-   License : GPL

#### Jekyll

-   <http://jekyllrb.com/>
-   Desc : a simple, blog-aware, static site generator.
-   License : The MIT License
-   Written in : Ruby

#### Tiki Wiki CMS Groupware

-   <http://info.tiki.org/>
-   Desc. : a free and open source wiki-based, content management system and Online office suite
-   License : LGPL 2.1
-   Written in : PHP

### Desktop Sharing

#### x11vnc

-   <http://www.karlrunge.com/x11vnc/>
-   Desc. : a VNC server for real X displays
-   License :
-   Readings
    -   [x11vnc command line options](http://www.karlrunge.com/x11vnc/x11vnc_opts.html)

#### TightVNC

-   <http://www.tightvnc.com/>
-   Desc. : a free remote control software package.
-   License : GPL

### BPM Engine

#### Activiti

-   <http://www.activiti.org/>
-   Desc : a light-weight workflow and Business Process Management (BPM) Platform targeted at business people, developers and system admins.
-   License : Apache License 2.0

### misc

#### Openfire

-   <http://www.igniterealtime.org/projects/openfire/>
-   Desc : a real time collaboration (RTC) server licensed under the Open Source Apache License.
-   License : Apache License

#### Supervisor

-   <http://supervisord.org/>
-   Desc. : a client/server system that allows its users to monitor and control a number of processes on UNIX-like operating systems.
-   License :
-   Written in : Python

#### Spring Loaded

-   <https://github.com/spring-projects/spring-loaded>
-   Desc. : a JVM agent for reloading class file changes whilst a JVM is running.
-   License : Apache License Version 2.0

#### Docker

-   <https://www.docker.com/>
-   Desc. : an open platform for developers and sysadmins to build, ship, and run distributed applications.
-   License : Apache License Version 2.0
-   Source : <https://github.com/docker/docker>

#### Vagrant

-   <https://www.vagrantup.com/>
-   Desc. : provides easy to configure, reproducible, and portable work environments built on top of industry-standard technology and controlled by a single consistent workflow to help maximize the productivity and flexibility of you and your team.
-   License :

Utilities
---------

### Browser

#### Firefox

-   Readings
    -   [Editing configuration](http://kb.mozillazine.org/Editing_configuration)
    -   [Managing bookmarks of Firefox](http://kb.mozillazine.org/Bookmarks)
    -   [Export bookmarks to bookmarks.html each time the browser shuts down](http://kb.mozillazine.org/Browser.bookmarks.autoExportHTML)

#### Thunderbird

-   Readings
    -   [Thunderbird and Exchange / OWA](http://thunderbirdtweaks.blogspot.com/2011/11/thunderbird-and-exchange-owa.html)
    -   [mozillaZine : Outlook Web Access](http://kb.mozillazine.org/Outlook_Web_Access)
    -   [Calendar : How do I save my calendar file on a local/network drive](https://wiki.mozilla.org/Calendar:FAQ#How_do_I_save_my_calendar_file_on_a_local.2Fnetwork_drive)

### PC Diagnostic and Recovery

#### GNU GRUB

-   <http://www.gnu.org/software/grub/>
-   Desc. : a Multiboot boot loader
-   License : GPL 3+

#### Hiren's BootCD

-   <http://www.hiren.info/pages/bootcd>
-   Desc. : A bootable software CD containing a number of diagnostic programs such as partitioning agents, system performance benchmarks, disk cloning and imaging tools, data recovery tools, MBR tools, BIOS tools, and many others for fixing various computer problems.
-   [fan & discussion site](http://www.hirensbootcd.org/) (for download and more information)
-   Articles
    -   [Launching Hiren's BootCD from USB Flash Drive](http://www.hirensbootcd.org/usb-booting/)

### File Synchronization/Backup

#### rsync

-   <http://rsync.samba.org/>
-   Desc. : An open source utility that provides fast incremental file transfer.
-   [Front-end soft-wares or solutions based on rsync](http://en.wikipedia.org/wiki/Rsync#Solutions_using_Rsync).

#### Partition Wizard

-   <http://www.partitionwizard.com/>

### Optical Disc Authoring

#### CDBurnerXP

-   [<http://cdburnerxp.se>](http://cdburnerxp.se)
-   Desc. : Application to burn CDs and DVDs, including Blu-Ray and HD-DVDs.
-   [License](http://cdburnerxp.se/help/intro/license)
    -   Limited grants you (the licensee) a permission to use the software at no cost, both for commercial and non-commercial purposes on any computer in your possession.

#### InfraRecorder

-   <http://infrarecorder.org/>
-   Desc. : A free CD/DVD burning solution for Microsoft Windows.
-   License : GPL
-   [InfraRecorder on Wikipedia](http://en.wikipedia.org/wiki/InfraRecorder)

### PC Security

#### Comodo Internet Security

-   <https://www.comodo.com/>
-   Desc. : an Internet security suite for Microsoft Windows including an antivirus program, a personal firewall, a sandbox and a host-based intrusion prevention system (HIPS)
-   [Comodo Internet Security on Wikipedia](https://en.wikipedia.org/wiki/Comodo_Internet_Security)

Image Editor
------------

### GIMP

-   <https://www.gimp.org/>
-   Desc. : a cross-platform image editor available for GNU/Linux, OS X, Windows and more operating systems.
-   License : GPL v3

<!-- -->

-   Readings
    -   [Official user manual](https://docs.gimp.org/2.8/en/)
    -   [Move Tool](https://docs.gimp.org/en/gimp-tool-move.html) : used to move layers, selections, paths or guides
    -   [Floating selection in Gimp](http://www.dreevoo.com/content.php?id=664)
    -   [Align Visible Layers](https://docs.gimp.org/en/plug-in-align-layers.html) : can very precisely position the visible layers
    -   [Removing Image Backgrounds - GIMP Fuzzy Select](http://gimptips.com/articles/removing-image-backgrounds-gimp-fuzzy-select) (31 Oct 2010)

### misc

#### LibreOffice

-   <https://www.libreoffice.org/>
-   Desc. : a powerful office suite
-   License : [Mozilla Public License v2.0](https://www.libreoffice.org/about-us/licenses/)

#### Notepad++

-   <https://notepad-plus-plus.org/>
-   Desc. : a free source code editor and Notepad replacement that supports several languages.
-   License : GPL

#### Excel

-   Readings
    -   [Excel Date & Time Calculations](http://www.ozgrid.com/Excel/date-time-calculations.htm)
    -   [Generate Gantt like chart in Excel](http://capzzang.tistory.com/14)(Korean)

#### CCleaner

-   <https://www.piriform.com/ccleaner>
-   Desc. : a utility program used to clean potentially unwanted files (including temporary internet files, where malicious programs and code tend to reside) and invalid Windows Registry entries from a computer.
-   [CCleaner on Wikipedia](https://en.wikipedia.org/wiki/CCleaner)

#### Picasa

-   Readings
    -   [How To Remove Picasa Duplicates and Hidden Junk?](http://www.apartmenttherapy.com/q-i-love-google-picasa-146888)
    -   [How to Delete Duplicate Pictures in Picasa](http://smallbusiness.chron.com/delete-duplicate-pictures-picasa-43919.html)

#### Hangul Typing

-   <http://www.hanfriends.com/content/services/game_typing.php>

misc
----

### Standards

#### eTOM

-   <https://www.tmforum.org/business-process-framework/>
-   Desc. : a business process framework for telecom service providers in the telecommunications industry.

<!-- -->

-   Readings
    -   [ITU-T Recommendation M.3050](https://www.itu.int/rec/T-REC-M.3050/en)

#### ONVIF

-   <http://www.onvif.org/>
-   Desc. : an open industry forum for the development of a global standard for the interface of IP-based physical security products.

<!-- -->

-   References
    -   **Specification**
        -   [ONVIF Specifications](http://www.onvif.org/Documents/Specifications.aspx)
        -   [ONVIF Core Specification, ver 16.12](http://www.onvif.org/specs/DocMap-16.12.html)
            -   Access Control, Access Rules, Action Engine, Advanced Security, Analytics, Credential, Device IO, Display, Door Control, Imaging, Media, Media2, Provisioning, PTZ, Receiver, Recording Control, Recording Search, Replay Control, Schedule, Thermal, Video Analytics Device
        -   [ONVIF Core Specification, ver 2.5](http://www.onvif.org/specs/core/ONVIF-Core-Specification-v250.pdf)(Dec, 2014)
            -   Device Discovery, Device Management, Event Framework
        -   [ONVIF Analytics Service Specification, ver 2.5](http://www.onvif.org/specs/srv/analytics/ONVIF-Analytics-Service-Spec-v250.pdf)(Dec, 2014)
        -   [ONVIF Recording Search Service Specification, ver 2.5](http://www.onvif.org/specs/srv/rsrch/ONVIF-RecordingSearch-Service-Spec-v250.pdf)(Dec, 2014)
    -   **Profile**
        -   [ONVIF Profile G Specification, ver 1.0](http://www.onvif.org/Portals/0/documents/specs/ONVIF_Profile_G_Specification_v1-0.pdf)(December 2013)
            -   Capabilities : `GetServices`, `GetServiceCapabilities`
            -   Recording Search : `FindRecordings`/`GetRecordingSearchResults`, `FindEvents`/`GetEventSearchResults`
            -   Recording Control : `CreateRecording`, `DeleteRecording`, `CreateTrack`, `DeleteTrack`
    -   **Schema**
        -   [ONVIF Schema, ver 2.5](http://www.onvif.org/onvif/ver10/schema/onvif.xsd)
        -   [OASIS WS-BaseNotification XMl Schema](http://docs.oasis-open.org/wsn/b-2.xsd)
    -   **WSLDs**
        -   [ONVIF 2.0 Service Operation Index](http://www.onvif.org/onvif/ver20/util/operationIndex.html)
        -   [ONVIF Device Management Service WSDL, ver 2.5](http://www.onvif.org/onvif/ver10/device/wsdl/devicemgmt.wsdl)
        -   [ONVIF Analytics Service WSDL, ver 2.2](http://www.onvif.org/onvif/ver20/analytics/wsdl/analytics.wsdl)
        -   [ONVIF Video Analytics Device\_Service WSDL, ver 2.1.1](http://www.onvif.org/onvif/ver10/analyticsdevice.wsdl)
        -   [ONVIF Recording Search Service WSDL, ver 2.4.2](http://www.onvif.org/onvif/ver10/search.wsdl)

<!-- -->

-   Search
    -   [`FindEvents`](http://www.onvif.org/onvif/ver10/search.wsdl#op.FindEvents)
        -   starts a search session, looking for recording events in the scope that matches the search filter defined in the request.
    -   [`GetEventSearchResults`](http://www.onvif.org/onvif/ver10/search.wsdl#op.GetEventSearchResults)
        -   acquires the results from a recording event search session previously initiated by a FindEvents operation.
    -   [`FindRecordings`](http://www.onvif.org/onvif/ver10/search.wsdl#op.FindRecordings)
        -   starts a search session, looking for recordings that matches the scope defined in the request.
    -   [`GetRecordingSearchResults`](http://www.onvif.org/onvif/ver10/search.wsdl#op.GetRecordingSearchResults)
        -   acquires the results from a recording search session previously initiated by a FindRecordings operation.
    -   [`FindMetadata`](http://www.onvif.org/onvif/ver10/search.wsdl#op.FindMetadata)
        -   starts a search session, looking for metadata in the scope that matches the search filter defined in the request.
    -   [`GetMetadataSearchResults`](http://www.onvif.org/onvif/ver10/search.wsdl#op.GetMetadataSearchResults)
        -   acquires the results from a recording search session previously initiated by a FindMetadata operation.

### Services

#### Amazon Redshift

-   <http://aws.amazon.com/redshift/>
-   Desc. : a fast, fully managed, petabyte-scale data warehouse solution that makes it simple and cost-effective to efficiently analyze all your data using your existing business intelligence tools.
-   Readings
    -   [Amazon Redshift Documentation](http://aws.amazon.com/documentation/redshift/)

#### Microsoft Project Oxford

-   <https://www.projectoxford.ai/>
-   Desc. : a set of services for understanding data and adding ‘smart’ to your applications.
-   Components
    -   Computer Vision APIs
    -   Face APIs
    -   Emotion APIs
    -   Speech APIs
    -   Spell Check APIs
    -   Language Understanding Intelligent Service

### Awards Winner

#### JAX Innovation Awards

-   [2016](https://jaxenter.com/winners-jax-innovation-awards-2016-jax-london-129588.html)

| Categories                                               | Winners       | Remarks |
|----------------------------------------------------------|---------------|---------|
| Most innovative contribution to the Java ecosystem       | Spring Boot   |         |
| Most innovative solution to software delivery and DevOps | Docker        |         |
| Special Jury Award                                       | Let’s Encrypt |         |

-   [2015](https://jaxenter.com/jax-awards-2015)

| Categories                      | Winners                                   | Remarks |
|---------------------------------|-------------------------------------------|---------|
| Most Innovative Java Technology | Java 8                                    |         |
| Most Innovative Open Tech       | [Akka](http://akka.io/)                   |         |
| Special Jury Award              | [Netflix OSS](https://netflix.github.io/) |         |

-   [2014](https://jaxenter.com/jax-innovation-awards-2014-champions-declared-107796.html)

| Categories                         | Winners                            | Remarks                                                                    |
|------------------------------------|------------------------------------|----------------------------------------------------------------------------|
| Most Innovative Java Technology    | [Vert.x](http://vertx.io/)         | a tool-kit for building reactive applications on the JVM                   |
| Most Innovative Open Technology    | [Docker](https://www.docker.com/)  | an open platform for distributed applications for developers and sysadmins |
| Most Innovative Open Tech Business | [Hazelcast](http://hazelcast.org/) | The Leading Open Source In-Memory Data Grid                                |

-   [2012](https://jaxenter.com/jax-innovation-awards-winners-revealed-104747.html)

| Categories                      | Winners                                     | Remarks            |
|---------------------------------|---------------------------------------------|--------------------|
| Most Innovative Java Technology | [Restructure101](https://structure101.com/) | commercial product |
| Most Innovative Java Company    | JetBrains                                   |                    |
| Top Java Ambassador             | Adam Bien                                   |                    |
| Special Jury Award              | Charlie Nutter                              |                    |

-   [2011](https://jaxenter.com/jax-innovation-awards-2011-the-winners-are-103447.html)

| Categories                      | Winners        | Remarks |
|---------------------------------|----------------|---------|
| Most Innovative Java Technology | JRebel         |         |
| Most Innovative Java Company    | Red Hat        |         |
| Top Java Ambassador             | Martin Odersky |         |
| Special Jury Award              | Brian Goetz    |         |

### Software Ranking

-   [GitHub/Popular Starred Repositories](https://github.com/popular/starred)

<!-- -->

-   [GitHub Showcases](https://github.com/showcases/)
    -   [GitHub/Showcases/Design essentials](https://github.com/showcases/design-essentials?s=stars)
    -   [GitHub/Showcases/Front-end JavaScript frameworks](https://github.com/showcases/front-end-javascript-frameworks?s=stars)
    -   [GitHub/Showcases/Data visualization tools](https://github.com/showcases/data-visualization?s=stars)
    -   [GitHub/Showcases/NoSQL databases](https://github.com/showcases/nosql-databases?s=stars)
    -   [GitHub/Showcases/DevOps tools](https://github.com/showcases/devops-tools?s=stars)
    -   [GitHub/Showcases/Emoji](https://github.com/showcases/emoji?s=stars)

### Graphics Asset

-   Tango Desktop Project : [<http://tango.freedesktop.org/>](http://tango.freedesktop.org/)
    -   Defines an icon style guideline to which artists and designers can adhere and provides a sample implementation of the style as an icon theme based upon a standardized icon naming specification.

<!-- -->

-   Silk Icons : [<http://www.famfamfam.com/lab/icons/silk/>](http://www.famfamfam.com/lab/icons/silk/)
    -   A smooth, free icon set, containing over 700 16-by-16 pixel icons in strokably-soft PNG format.
-   Liquid Look And Feel : [<http://sourceforge.net/projects/liquidlnf/>](http://sourceforge.net/projects/liquidlnf/)
    -   Java2 Swing Look and Feel of Mosfet Liquid KDE 3.x theme.

<!-- -->

-   Open Source Web Design : [<http://www.oswd.org/>](http://www.oswd.org/)
    -   A site to download free web design templates and share yours with others.

<!-- -->

-   IconFinder : <http://www.iconfinder.com/>
    -   provides high quality icons for webdesigners and developers in an easy and efficient way.

