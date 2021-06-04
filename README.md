# e.net
easy .net , collections of .net


1. log4net
log4net.config
```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <configSections>
    <section name="log4net" type="log4net.Config.Log4NetConfigurationSectionHandler, log4net" />
  </configSections>
  <!--log4net配置-->
  <log4net debug="false">
    <appender name="rollFile" type="Log4Net.Async.AsyncRollingFileAppender,Log4Net.Async">
      <file value="logs/log_" />
      <appendToFile value="false" />
      <staticLogFileName value="false" />
      <rollingStyle value="Date" />
      <!--混合使用日期和文件大小变换日志文件名-->
      <!--<rollingStyle value="Composite" />-->
      <!--日期的格式-->
      <datePattern value="yyyyMMdd'.log'" />
      <param name="Encoding" value="utf-8" />
      <!--最大变换数量-->
      <maxSizeRollBackups value="100" />
      <!--最大文件大小-->
      <maximumFileSize value="10MB" />
      <layout type='log4net.Layout.SerializedLayout, log4net.Ext.Json'>
        <decorator type='log4net.Layout.Decorators.StandardTypesDecorator, log4net.Ext.Json' />
        <default />
        <remove value='message' />
        <remove value='ndc' />
        <remove value='logger' />
        <member value='message:messageobject' />
      </layout>
    </appender>
    <root>
      <appender-ref ref="rollFile" />
    </root>
  </log4net>
</configuration>
```

2. nlog
NLog.config
```xml
<?xml version="1.0" encoding="utf-8" ?>
<nlog autoReload="true" throwExceptions="true"
      xmlns="http://www.nlog-project.org/schemas/NLog.xsd" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
  <variable name="LogDir" value="${basedir}/logs"/>
  <variable name="LogDay" value="${date:format=yyyyMMdd}"/>
  <variable name="jLayout" value='{ "date":"${longdate}","level":"${level}","message":${message}}' />
  <targets>
    <!--layout="${json-encode} ${message}" -->
    <target name="allfile"
            xsi:type="File"
            fileName="${LogDir}/${LogDay}.log"
            encoding="utf-8"
            maxArchiveFiles="10"
            archiveEvery="Day"
            archiveNumbering="DateAndSequence"
            archiveAboveSize="10485760"
            archiveFileName="${LogDir}/{#}.a"
            layout="${jLayout}"
            />
      <!--<layout xsi:type="JsonLayout" includeAllProperties="true" excludeProperties="Comma-separated list (string)">
        <attribute name="time" layout="${longdate}" />
        <attribute name="level" layout="${level:upperCase=true}"/>
        <attribute name="message" layout="${json-encode} ${message}" />
      </layout>
    </target>-->
  </targets>
  <rules>
      <!--All logs, including from Microsoft-->
      <logger name="*" minlevel="Info" writeTo="allfile" />
    </rules>
</nlog>
```