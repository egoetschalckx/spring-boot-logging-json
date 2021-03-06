[![Build Status](https://travis-ci.org/goetschalckx/spring-boot-logging-json.svg?branch=master)](https://travis-ci.org/goetschalckx/spring-boot-logging-json)
[![License](https://img.shields.io/github/license/goetschalckx/spring-boot-logging-json?color=4DC71F)](https://github.com/goetschalckx/spring-boot-logging-json/blob/master/LICENSE)

[![Coverage](https://codecov.io/gh/goetschalckx/spring-boot-logging-json/branch/master/graph/badge.svg)](https://codecov.io/gh/goetschalckx/spring-boot-logging-json)
[![Codacy](https://app.codacy.com/project/badge/Grade/39ea34a49b254b03bf84d5d1adbec00a)](https://www.codacy.com/gh/goetschalckx/spring-boot-logging-json?utm_source=github.com&amp;utm_medium=referral&amp;utm_content=goetschalckx/spring-boot-logging-json&amp;utm_campaign=Badge_Grade)
[![Codacy Coverage](https://app.codacy.com/project/badge/Coverage/39ea34a49b254b03bf84d5d1adbec00a)](https://www.codacy.com/gh/goetschalckx/spring-boot-logging-json?utm_source=github.com&utm_medium=referral&utm_content=goetschalckx/spring-boot-logging-json&utm_campaign=Badge_Coverage)
[![CodeFactor](https://www.codefactor.io/repository/github/goetschalckx/spring-boot-logging-json/badge)](https://www.codefactor.io/repository/github/goetschalckx/spring-boot-logging-json)
[![Vulnerabilities](https://snyk.io/test/github/goetschalckx/spring-boot-logging-json/badge.svg)](https://snyk.io/test/github/goetschalckx/spring-boot-logging-json)

[![Release](https://img.shields.io/nexus/r/io.github.goetschalckx/spring-boot-logging-json?color=4DC71F&label=release&server=https%3A%2F%2Foss.sonatype.org%2F)](https://search.maven.org/artifact/io.github.goetschalckx/spring-boot-logging-json)
[![Snapshot](https://img.shields.io/nexus/s/io.github.goetschalckx/spring-boot-logging-json?label=snapshot&server=https%3A%2F%2Foss.sonatype.org%2F)](https://oss.sonatype.org/#nexus-search;quick~spring-boot-logging-json)

# spring-boot-logging-json
by Eric Goetschalckx

[json](https://github.com/json/json-spec) structured JSON log support for Spring Boot applications.

This capability works best in environments using log aggregation tools like [Stackdriver](https://cloud.google.com/stackdriver) or [ELK](https://www.elastic.co/what-is/elk-stack).

When enabled, each log event emitted is a minified JSON object containing the log message and other metadata. 

Unlocks more potential from the Logstash Logback JSON encoder.

Having metadata in discrete JSON fields allows for advanced queries and visualizations in log aggregation UIs.

Uses high-performance [LMAX Async Disruptor](https://github.com/LMAX-Exchange/disruptor) console appender.

## Simple Sample Log Statement
```json
{
  "log" : "c.g.s.log.json.TestApplication",
  "lvl" : "INFO",
  "thread" : "main",
  "msg" : "log examples {logstash.argument=example}",
  "ts" : "2019-11-13T01:24:51.200+00:00",
  "HOSTNAME" : "dev",
  "logstash.argument" : "example",
  "logstash.marker" : {
    "json" : "example"
  },
  "tags" : [ "slf4j.marker" ],
  "mdc" : "example"
}
```

Top-level JSON fields can be added using:
-   [SLF4J MDC](http://www.slf4j.org/api/org/slf4j/MDC.html)
-   [Logstash Markers](https://github.com/logstash/logstash-logback-encoder/blob/master/src/main/java/net/logstash/logback/marker/Markers.java)
-   [Logstash Arguments](https://github.com/logstash/logstash-logback-encoder/tree/master/src/main/java/net/logstash/logback/argument)
-   [SLF4J Markers](https://github.com/qos-ch/slf4j/tree/master/slf4j-api/src/main/java/org/slf4j)
    -   This populates the top-level JSON field `tags` seen in the sample log statement above.

## Minimum Requirements
Requires 
-   `spring-boot` v1.4.0.RELEASE+
-   `logstash-logback-encoder` v6.3+

## Spring Profiles
Use the following Spring profile(s) to enable the various logging output options.

-   `default` or `log-dev`
    -   "Standard" Spring logging, colorized, pretty-printed, etc...
    -   No support for custom fields.
-   `log-json`
    -   json structured logs to the console.
    -   Supports custom JSON fields.
    -   Intended for containerized deployments.

### Profiles Drawback
The problem with this approach is that if you already use a custom Spring Profile besides default, you will get no logs, which is less than ideal.

## Configuration
The following configuration properties are available for managing logging settings:

```yaml
# Logging settings 
logging:
  
  # Newline-delimited JSON Logging Settings
  json:
      
    # Enable JSON pretty-print
    # Optional (default is false)
    pretty-print: false
```
