#Log4Net.config#

Log4Net is a third-party text file logging plugin that logs information to `~/App_Data/Logs`.  You can control the behavior by altering this file. 

```xml
<?xml version="1.0"?>
<log4net>
  
  <root>
    <priority value="Info"/>
    <appender-ref ref="AsynchronousLog4NetAppender" />
  </root>
  
  <appender name="AsynchronousLog4NetAppender" type="Umbraco.Core.Logging.AsynchronousRollingFileAppender, Umbraco.Core">
    <file value="App_Data\Logs\UmbracoTraceLog.txt" />
    <lockingModel type="log4net.Appender.FileAppender+MinimalLock" />
    <appendToFile value="true" />
    <rollingStyle value="Date" />
    <maximumFileSize value="5MB" />
    <layout type="log4net.Layout.PatternLayout">
      <conversionPattern value="%date [%thread] %-5level %logger - %message%newline" />
    </layout>
    <encoding value="utf-8" />
  </appender>

  <!--Here you can change the way logging works for certain namespaces  -->

  <logger name="NHibernate">
    <level value="WARN" />
  </logger>
</log4net>
```

For more information about Log4Net, please visit the [official documentation](https://logging.apache.org/log4net/).

[<Back 06 - FileSystemProviders.config](06 - FileSystemProviders.config.md)

[Next> 08 - UmbracoSettings.config](08 - UmbracoSettings.config.md)