

````markdown
# 🔍 Custom Analytics Data Provider for WSO2 API Manager

This sample project demonstrates how to add **custom analytics data** to the event schema in **WSO2 API Manager** by implementing a custom data provider.

---

## 🚀 Features

- Plug-and-play custom data provider
- Seamless integration with WSO2 API Manager analytics
- Compatible with trace logging and metrics monitoring

---

## 📦 Prerequisites

- Java 
- Maven  
- WSO2 API Manager (matching your component versions)

---

## ⚙️ Setup Instructions

### 1. Clone the Repository
`
````

### 2. Update Dependency Versions

In the root `pom.xml`, verify and set correct versions for:

```xml
<carbon.apimgt.version>YOUR_VERSION</carbon.apimgt.version>
<synapse.version>YOUR_VERSION</synapse.version>
```

Make sure they match your WSO2 API Manager installation.

### 3. Build the Project

```bash
mvn clean install
```

This will generate a JAR file inside the `target/` directory.

### 4. Deploy the JAR

Copy the built JAR into the WSO2 API Manager’s library:

```bash
cp target/*.jar <WSO2AM_HOME>/repository/components/lib/
```

---

## 🔧 Configuration

Edit the file:

```ini
<WSO2AM_HOME>/repository/conf/deployment.toml
```

Add or modify the following section:

```toml
[apim.analytics]
enable = true
type = "elk"
properties."publisher.custom.data.provider.class" = "org.wso2.carbon.apimgt.gateway.sample.publisher.CustomDataProvider"
```

---

## 🧪 Verifying the Setup

### 1. Enable Trace Logs

Follow WSO2’s [logging guide](https://apim.docs.wso2.com/en/latest/administer/logging-and-monitoring/logging/configuring-logging/#enabling-logs-for-a-component) and append this to your log4j2 configuration:

```properties
loggers = org-wso2-analytics-publisher , trace-messages

logger.org-wso2-analytics-publisher.name = org.wso2.am.analytics.publisher
logger.org-wso2-analytics-publisher.level = TRACE
logger.org-wso2-analytics-publisher.appenderRef.CARBON_TRACE_LOGFILE.ref = CARBON_TRACE_LOGFILE
```

Check logs under:

```bash
<WSO2AM_HOME>/repository/logs/wso2carbon-trace-messages.log
```

### 2. (Optional) Log to `apim_metrics.log`

If desired, add the following:

```properties
loggers = reporter,

logger.reporter.name = org.wso2.am.analytics.publisher.reporter.elk
logger.reporter.level = INFO
logger.reporter.additivity = false
logger.reporter.appenderRef.APIM_METRICS_APPENDER.ref = APIM_METRICS_APPENDER
```

To avoid duplication, remove this if you only want logs in the trace log. (enabled by default))

