# Monitor contension

## Description

In charge of monitoring service's invocation statistics.

## Interface

* `com.alibaba.dubbo.monitor.MonitorFactory`
* `com.alibaba.dubbo.monitor.Monitor`

## Configuration

```xml
<!-- configure monitor -->
<dubbo:monitor address="xxx://ip:port" />
```

## Known extension

com.alibaba.dubbo.monitor.support.dubbo.DubboMonitorFactory

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxMonitorFactoryjava (implement MonitorFactory)
                |-XxxMonitor.java (implement Monitor)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.monitor.MonitorFactory (text file with content of xxx=com.xxx.XxxMonitorFactory)
```

XxxMonitorFactory.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.monitor.MonitorFactory;
import com.alibaba.dubbo.monitor.Monitor;
import com.alibaba.dubbo.common.URL;
 
public class XxxMonitorFactory implements MonitorFactory {
    public Monitor getMonitor(URL url) {
        return new XxxMonitor(url);
    }
}
```

XxxMonitor.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.monitor.Monitor;
 
public class XxxMonitor implements Monitor {
    public void count(URL statistics) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.monitor.MonitorFactory：

```properties
xxx=com.xxx.XxxMonitorFactory
```