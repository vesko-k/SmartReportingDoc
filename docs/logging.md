**Smart Reporting** uses **NLog** platform for message logging. **NLog**
attempts to automatically configure itself on startup, by looking for
the configuration files. All configuration of the **NLog** is situated
in the "**&lt;InstallDir&gt;\\NLog.config**" configuration file.

# Configuration File Format

The simplified format is the pure **XML** having the `<nlog \>`
element as its root. The use of namespaces is optional.

```xml
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
</nlog>
```

Note that "**NLog.config**" file is case-insensitive when not using
namespaces and is case-sensitive when you use them.

The following elements are children of `<nlog />`. The first two
elements from the list are required to be present in all **NLog**
configuration files, the remaining ones are optional and can be useful
in advanced scenarios.

-   `<targets />` - defines log targets/outputs

-   `<rules />` - defines log routing rules

-   `<extensions />` - loads **NLog** extensions from the \*.dll
    file

-   `<include />` - includes external configuration file

-   `<variable />` - sets the value of a configuration variable

# Targets

The `<targets />` section defines log
[Targets](https://github.com/nlog/nlog/wiki/Targets). Each target is
represented by an element. There are two attributes required for each
target:

- ***name*** - target name;

- ***type*** - target type - such as "**File**", "**Database**",
    "**Mail**", "**NLogViewer**", "**Syslog**". When using namespaces
    this attribute is named `xsi:type`.

In addition to these attributes, targets usually accept other
parameters, which impact the way diagnostic traces are written. Each
target has a different set of parameters and they are context-sensitive.

For example - the [File Target](https://github.com/nlog/nlog/wiki/File%20Target) accepts
the `fileName` parameter which defines output file name and the [Console Target](https://github.com/nlog/nlog/wiki/Console%20Target) has
the `error` parameter which determines whether the diagnostic traces are
written to standard error (**stderr**) instead of standard output (**stdout**)  the process.

This example demonstrates a `<targets />` section which defines
multiple targets: two files, one network target and `OutputDebugString`
target:

```xml
<targets>
  <target name="f1" xsi:type="File" fileName="file1.txt"/>
  <target name="f2" xsi:type="File" fileName="file2.txt"/>  
  <target name="n1" xsi:type="Network" address="tcp://localhost:4001"/>
  <target name="ds" xsi:type="OutputDebugString"/>
</targets>
```

**NLog** provides many predefined [Targets](https://github.com/nlog/nlog/wiki/Targets).

# Rules

Log routing rules are defined in the `<rules />` section. It is a
simple routing table, where we define the list of targets that should be
written to for each combination of source/logger name and log level.
Rules are processed starting with the first rule in the list. When a
rule matches, log messages are directed to target(s) in that rule. If a
rule is marked as final, rules below it are not processed.

Each routing table entry is a `<logger />` element, which accepts
the following attributes:

- ***name*** - source/logger name (may include wildcard characters \*)

- ***minlevel*** - minimal log level for this rule to match

- ***maxlevel*** - maximum log level for this rule to match

- ***level*** - single log level for this rule to match

- ***levels*** - comma separated list of log levels for this rule to match

- ***writeTo*** - comma separated list of target that should be written 
to when this rule matches.

- ***final*** - make this rule final. No further rules are processed when
any final rule matches.

- ***enabled*** - setting enabled to false allows to disable this rule.
Disabled rules are ignored.

The level related keywords are processed in the following order:

1.  level

2.  levels

3.  minlevel and maxlevel (minimum and maximum level keywords have the
    same priority)

4.  No keyword (All levels are logged)

# Layouts and Layout Renderers
        
One of **NLog**'s strongest assets is the ability to use
[layouts](https://github.com/nlog/nlog/wiki/Layouts). In the [simplest form](https://github.com/nlog/nlog/wiki/Simple%20Layout) layouts are
texts with embedded tags delimited by \${ and }. The tags are called
[Layout Renderers](https://github.com/nlog/nlog/wiki/Layout%20Renderers)
and can be used to insert pieces of contextual information into the
text.

Layouts can be used in many places, for example they can control the
format of information written on the screen or sent to a file, but also
to control the file names themselves.

Let's assume, that we want to annotate each message written to the
console with:

- current date and time

- name of the class and method that emitted the log message

- log level

- message text

This is very easy:

```xml
<target name="c" xsi:type="Console"  layout="${longdate} ${callsite} ${level} ${message}"/>
```

We can make each messages for each logger go to a separate file, as in
the following example:

```xml
<target name="f" xsi:type="File" fileName="${logger}.txt"/>
```

# Include Files

It's sometimes desired to split the configuration file into many smaller
ones. **NLog** provides an include file mechanism for that. To include
an external file, you simply use `<include file="..." />` element.
It's worth noting that the file attribute, just like most attributes in
**NLog** config file(s), may include dynamic values using the familiar
`${}` notation for layout renderers, so it's possible to include
different files based on environmental properties.

The following configuration example demonstrates this, by loading a file
whose name is derived from the name of the machine we're running on.

```xml
<nlog>
  ...
  <include file="${basedir}/${machinename}.config"/>
  ...
</nlog>
```

The optional attribute, `ignoreErrors="true"`, which defaults to
`false`, can be added to prevent an exception from being thrown when
the include file is not found or is not formatted correctly. When
setting `ignoreErrors="true"`, use the [Troubleshooting Logging](https://github.com/nlog/nlog/wiki/Configuration-file#troubleshooting-logging) section to log errors.

# Variables

Variables can be used to write complex or repeated expressions (such as
file names) in a concise manner. To define a variable use the following
syntax:

```xml
<variable name="var" value="xxx" />
```

Once defined, variables can be used as if they were layout renderers -
by using `${var}` syntax, as demonstrated in the following example:

```xml
<nlog>
  <variable name="logDirectory" value="${basedir}/logs/${shortdate}"/>
  <targets>
    <target name="file1" xsi:type="File" fileName="${logDirectory}/file1.txt"/>
    <target name="file2" xsi:type="File" fileName="${logDirectory}/file2.txt"/>
  </targets>
</nlog>
```

# Automatic Reconfiguration

The configuration file is read automatically at program startup. In a
long running process (such as a **Windows** service or an **ASP.NET**
application) it's sometimes desirable to temporarily increase the log
level without stopping the application. **NLog** can monitor logging
configuration files and re-read them each time they are modified. To
enable this mechanism, you simply
add `autoReload="true"` parameter to the configuration file.

```xml
<nlog autoReload="true">
    ...
</nlog>
```

Note that automatic reconfiguration supports include files, so each time
one of the included files is changed, the entire configuration gets
reloaded.

# Troubleshooting

Sometimes our application doesn't write anything to the log files, even
though we have supposedly configured logging properly. There can be many
reasons for logs not being written. The most common problems are
permissions issues, usually the process may not have write access to the
directory where we want to store logs.

**NLog** is designed to swallow run-time exceptions that may result from
logging. The following settings can change this behavior and/or redirect
these messages.

- `<nlog throwExceptions="true" />` - adding
the **throwExceptions** attribute in the config file causes **NLog**
to stop masking exceptions and pass them to the calling application
instead. This attribute is useful at deployment time to quickly
locate any problems. It's recommended to
set **throwExceptions** to **\"false\"** as soon as the application
is properly configured to run, so that any accidental logging
problems won't crash the application.

- `<nlog internalLogFile="file.txt" />` -
adding **internalLogFile** causes **NLog** to write its internal
debugging messages to the specified file. This includes any
exceptions that may be thrown during logging.

-  `<nlog internalLogLevel="Trace|Debug|Info|Warn|Error|Fatal"/>` - 
determines the internal log level. The higher the level, the less verbose 
the internal log output.

- `<nlog internalLogToConsole="false|true" />` - determines whether 
internal logging messages are sent to the console.

- `<nlog internalLogToConsoleError="false|true" />` -
determines whether internal logging messages are sent to the console
error output (stderr).

# Asynchronous Processing and Wrapper Targets

**NLog** provides wrapper and compound targets which modify other
targets' behavior by adding features like:

- asynchronous processing (wrapped target runs in a separate thread)

- retry-on-error

- load balancing

- buffering

- filtering

- failover (failover)

To define a wrapper in the configuration file, simply nest a target node
within another target node. You can even wrap a wrapper target - there
are no limits on depth. For example, to add asynchronous logging with
retry-on-error functionality add this to your configuration file:

```xml
<targets>
  <target name="n" xsi:type="AsyncWrapper">
    <target xsi:type="RetryingWrapper">
      <target xsi:type="File" fileName="${file}.txt" />
    </target>
  </target>
</targets>
```
Because asynchronous processing is a common scenario, **NLog** supports
a shorthand notation to enable it for all targets without the need to
specify explicit wrappers. You can simply set **async=\"true\"** on
targets element and all your targets within that element will be wrapped
with the AsyncWrapper target.

```xml
<nlog>
  <targets async="true">
    <!-- all targets in this section will automatically be asynchronous -->
  </targets>
</nlog>
```

*``Remark:`` We do not recommend to use `async` option.*

# Default wrappers

Sometimes we require all targets to be wrapped in the same way, for
example to add buffering and/or retrying. **NLog**
provides `<default-wrapper />` syntax for that. You simply put this
element in the `<targets />` section and all your targets will be
automatically wrapped with the specified wrapper. Note
that `<default-wrapper />` applies to the single `<targets/>` section only and you can have multiple sections so you can define
groups of targets that are wrapped in a similar manner.

```xml
<nlog>  
  <targets>  
    <default-wrapper xsi:type="BufferingWrapper" bufferSize="100"/>  
    <target name="f1" xsi:type="File" fileName="f1.txt"/>  
    <target name="f2" xsi:type="File" fileName="f2.txt"/>  
  </targets>  
  <targets>  
    <default-wrapper xsi:type="AsyncWrapper">  
      <wrapper-target xsi:type="RetryingWrapper"/>  
    </default-wrapper>  
    <target name="n1" xsi:type="Network" address="tcp://localhost:4001"/>  
    <target name="n2" xsi:type="Network" address="tcp://localhost:4002"/>  
    <target name="n3" xsi:type="Network" address="tcp://localhost:4003"/>  
  </targets>  
</nlog>
```

In the above example we've defined two buffered File targets and three
asynchronous and retrying Network targets.

# Default Target Parameters

Similar to default wrappers, **NLog**
provides `<default-target-parameters />` which enables you to
specify default values of target parameters. For example, if you don't
want files to be kept open, you can either
add `keepFileOpen="false"` to each target, as in the following
example:

```xml
<nlog>
  <targets>
    <target name="f1" xsi:type="File" fileName="f1.txt" keepFileOpen="false"/>
    <target name="f2" xsi:type="File" fileName="f2.txt" keepFileOpen="false"/>
    <target name="f3" xsi:type="File" fileName="f3.txt" keepFileOpen="false"/>
   </targets>
</nlog>
```

Alternatively you can specify a single `<default-target-parameters />` that applies to all targets in the `<targets />` section.
Default parameters are defined on a per-type basis and are applied
**BEFORE** the actual attributes defined in the XML file:

```xml
<source lang="xml">
<nlog>
  <targets>
    <default-target-parameters xsi:type="File" keepFileOpen="false"/>
    <target name="f1" xsi:type="File" fileName="f1.txt"/>
    <target name="f2" xsi:type="File" fileName="f2.txt"/>
    <target name="f3" xsi:type="File" fileName="f3.txt"/>
  </targets>
</nlog>
```

# Log levels

**NLog** supports the following [log levels](https://github.com/NLog/NLog/wiki/Log-levels):

- **Off**

- **Trace** - very detailed logs, which may include high-volume
information such as protocol payloads. This log level is typically
only enabled during development

- **Debug** - debugging information, less detailed than trace,
typically not enabled in production environment.

- **Info** - information messages, which are normally enabled in
production environment

- **Warn** - warning messages, typically for non-critical issues,
which can be recovered or which are temporary failures

- **Error** - error messages

- **Fatal** - very serious errors

Log Levels in **NLog** are consistent with **Log4j/Log4net**, so a
description of the different levels\' usage on [Wikipedia\'s Entry About log4j](http://en.wikipedia.org/wiki/Log4j) is applicable for NLog Log
Levels.

# NLog.config

The **NLog** configuration file can be found in the
"**&lt;InstallDir&gt;\\NLog.config**".

We have custom extension configured with section:

```xml
<extensions>
    <add assembly="Smart.OpcXml.NLogExtension"/>
</extensions>
```

The **Smart.OpcXml.NLogExtensions** brings additional features to
looging like **ColoredSharedConsole**, **SharedConsole**
**SharedMessage** and **Syslog** target types. Also in configuration
file there are several variables defined:

```xml
<variable name="fileext" value=".log" />
<variable name="simpleLayout" value="${date:format=yyyy-MM-dd HH\:mm\:ss} ${level} ${logger:shortName=true} ${message} ${exception:format=ToString}"/>
<variable name="intermeidiateLayout" value="${date:format=yyyy-MM-dd HH\:mm\:ss} ${processid} ${threadid} ${level} ${logger:shortName=true} ${message} ${exception:format=ToString} ${event-context:item=ADEHStackTrace}"/>
<variable name="advancedLayout" value="${longdate} ${processid} ${threadid} ${level} ${logger} ${callsite} ${message} ${stacktrace} ${exception:format=ToString} ${event-context:item=ADEHStackTrace}"/>  
```

The first one `fileext` defines file extension variable used for
defining file extension of the log files. The next 3 variables
`simpleLayout`, `intermediateLayout` and `advancedLayout`
defines 3 types of layouts which you can use directly.

Also we have several targets defined:

```xml
<targets>
<!—
<target name="ColoredSharedConsole" xsi:type="ColoredSharedConsole" 
useDefaultRowHighlightingRules="true"    footer="" header="" errorStream="false" layout="${simpleLayout}" >
<highlight-row backgroundColor="NoChange" condition="level == LogLevel.Info" foregroundColor="Cyan"/>
    </target>
    -->
<!—
<target name="SharedConsole" xsi:type="SharedConsole" footer="" header="" error="false"   layout="${simpleLayout}" />
    -->
    <target name="Console" xsi:type="Console" footer="" header="" error="false" layout="${simpleLayout}" />
    <target name="Trace" xsi:type="Trace" layout="${simpleLayout}" />
  </targets>
```

`ColoredSharedConsole` target of type `ColoredSharedConsole`, which
writes colored messages to the console output, `SharedConsole` target
of type `SharedConsole`, which writes non-colored messages to the
console output and so on. After that we can see another set of defined
targets:

```xml
<?xml version="1.0" ?>
<nlog xmlns="http://www.nlog-project.org/schemas/NLog.xsd"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" autoreaload="true" internalLogLevel="Error" internalLogToConsole="True" internalLogFile="Logs\NLog.log">
  <extensions>
    <add assembly="Smart.Reporting.NLogExtension"/>
  </extensions>

  <variable name="fileext" value=".log" />

  <variable name="simpleLayout" value="${date:format=yyyy-MM-dd HH\:mm\:ss} ${level} ${logger:shortName=true} ${message} ${exception:format=ToString}"/>
  <variable name="intermeidiateLayout" value="${date:format=yyyy-MM-dd HH\:mm\:ss} ${processid} ${threadid} ${level} ${logger:shortName=true} ${message} ${exception:format=ToString} ${event-context:item=ADEHStackTrace}"/>
  <variable name="advancedLayout" value="${longdate} ${processid} ${threadid} ${level} ${logger} ${callsite} ${message} ${stacktrace} ${exception:format=ToString} ${event-context:item=ADEHStackTrace}"/>

  <targets>
    <!--
    <target name="SharedConsole" xsi:type="ColoredSharedConsole" useDefaultRowHighlightingRules="true" footer="" header="" errorStream="false"
      layout="${advancedLayout}" >
      <highlight-row backgroundColor="NoChange" condition="level == LogLevel.Debug" foregroundColor="Cyan"/>
    </target>
    -->

    <!--
    <target name="SharedConsole" xsi:type="SharedConsole" footer="" header="" error="false"
      layout="${advancedLayout}" >
    </target>
    -->

    <target name="Console" xsi:type="Console" footer="" header="" error="false"
      layout="${simpleLayout}" >
    </target>

    <target name="Trace" xsi:type="Trace"
      layout="${advancedLayout}" />
  </targets>

  <targets >
    <default-target-parameters xsi:type="File"
      fileName="${basedir}/Logs/${filesystem-normalize:inner=${logger}}${fileext}"
      archiveFileName="${basedir}/Logs/Archives/${filesystem-normalize:inner=${logger}}-{##}${fileext}"
      createDirs="true"
      archiveNumbering="Rolling"
      archiveAboveSize="10000000"
      maxArchiveFiles = "14"
      concurrentWrites="true"
      keepFileOpen="false" >
      <layout xsi:type="CSVLayout" delimiter="Tab">
        <column name="Time" layout="${longdate}" />
        <column name="ProcessId" layout="${processid}" />
        <column name="ThreadId" layout="${threadid}" />
        <column name="Level" layout="${level}"/>
        <column name="Logger" layout="${logger}"/>
        <column name="CallSite" layout="${callsite}"/>
        <column name="Message" layout="${message}" />
        <column name="StackTrace" layout="${stacktrace}"/>
        <column name="Exception" layout="${exception:format=ToString}" />
        <column name="ADEHStackTrace" layout="${event-context:item=ADEHStackTrace}" />
      </layout>
    </default-target-parameters>

    <target name="NLogViewer" xsi:type="NLogViewer" address="udp://127.0.0.1:9999" onOverflow="Split">
      <parameter name="Time" layout="${longdate}" />
      <parameter name="MachineName" layout="${machinename}" />
      <parameter name="ProcessId" layout="${processid}" />
      <parameter name="ThreadId" layout="${threadid}" />
      <parameter name="Level" layout="${level}"/>
      <parameter name="Logger" layout="${logger}"/>
      <parameter name="CallSite" layout="${callsite}"/>
      <parameter name="Message" layout="${message}" />
      <parameter name="Exception" layout="${exception:format=ToString}" />
      <parameter name="StackTrace" layout="${stacktrace}" />
      <parameter name="ADEHStackTrace" layout="${event-context:item=ADEHStackTrace}" />
    </target>

    <!--
    <target name="database" xsi:type="Database">
      <dbprovider>mssql</dbprovider>
      <dbhost>.\SQLEXPRESS</dbhost>
      <dbdatabase>SmartReporting</dbdatabase>
      <dbusername>sa</dbusername>
      <dbpassword>sa123</dbpassword>
      <commandText>
        INSERT INTO SmartReportingLog ([LongDate],[ProcessId],[ThreadId],[LogLevel],[LoggerName],[CallSite],[Message],[Exception],[StackTrace],[ADEHStackTrace],[MachineName]) VALUES (@LongDate,@ProcessId,@ThreadId,@LogLevel,@LoggerName,@CallSite,@Message,@Exception,@StackTrace,@ADEHStackTrace,@MachineName);
      </commandText>

      <parameter name="@LongDate" layout="${longdate}" />
      <parameter name="@MachineName" layout="${machinename}" />
      <parameter name="@ProcessId" layout="${processid}" />
      <parameter name="@ThreadId" layout="${threadid}" />
      <parameter name="@LogLevel" layout="${level}"/>
      <parameter name="@LoggerName" layout="${logger}"/>
      <parameter name="@CallSite" layout="${callsite}"/>
      <parameter name="@Message" layout="${message}" />
      <parameter name="@Exception" layout="${exception:format=ToString}" />
      <parameter name="@StackTrace" layout="${stacktrace}" />
      <parameter name="@ADEHStackTrace" layout="${event-context:item=ADEHStackTrace}" />
    </target>
    -->

    <!--
    <target
      removenewlines="true"
      minlevel="Debug"
      name="Syslog"
      type="Syslog"
      address="udp://127.0.0.1:514"
      facility="Local6"
      sender="srpt${logger}[${processid}]"
      senderRemovePattern="Smart.Reporting.|Reporting|Services"
      senderReplacePattern="(\.AddIn\.|\.)"
      senderReplaceString="_"
      layout="${message} ${exception:format=ToString} ${event-context:item=ADEHStackTrace}" />
    -->

    <target name="Smart.Reporting.PhdReportingServices" xsi:type="File"/>
  </targets>

  <rules>
    <logger name="Smart.Reporting.PhdReportingServices" minlevel="Debug" writeTo="Smart.Reporting.PhdReportingServices,NLogViewer" />
  </rules>
</nlog>
```

All targets above have default target parameters of target type
`File` which shows that all targets in this group with type
`File` will inherit these default parameters. Also in this group we
have a target with name `NLogViewer` used to send `log4j` xml format
messages through a network and `Syslog` target used to send syslog
[RFC 3164](https://www.ietf.org/rfc/rfc3164.txt) messages through a
network.

After that we have defined a set of rules:

```xml
<rules>
    <logger name="Smart.Reporting.PhdReportingServices" minlevel="Debug" writeTo="Smart.Reporting.PhdReportingServices,NLogViewer" />
</rules>
```

This rules specifies depending of required logger, where the logger will
write its messages. The `name` attribute of the `<logger />`,
supports widlcards.

Rules loggers' names:

- ***Smart.Reporting.PhdReportingServices*** - Used by **Smart Reporting Service**;

# Wiki
More information about **NLog** can be found at <https://github.com/NLog/NLog/wiki>.