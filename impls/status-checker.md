# State check extension

## Description

The check service depends on the state of a variety of resources, which can be used at the same time for the status command of telnet and the status page of the hosting.

## Interface

`com.alibaba.dubbo.common.status.StatusChecker`

## Configuration

```xml
<dubbo:protocol status="xxx,yyy" />
<!-- Default setting, when <dubbo:protocol> does not configure the status property, use this configuration -->
<dubbo:provider status="xxx,yyy" />
```

## Known extensions

* `com.alibaba.dubbo.common.status.support.MemoryStatusChecker`
* `com.alibaba.dubbo.common.status.support.LoadStatusChecker`
* `com.alibaba.dubbo.rpc.dubbo.status.ServerStatusChecker`
* `com.alibaba.dubbo.rpc.dubbo.status.ThreadPoolStatusChecker`
* `com.alibaba.dubbo.registry.directory.RegistryStatusChecker`
* `com.alibaba.dubbo.rpc.config.spring.status.SpringStatusChecker`
* `com.alibaba.dubbo.rpc.config.spring.status.DataSourceStatusChecker`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxStatusChecker.java (implement StatusChecker)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.common.status.StatusChecker (Pure text file, content: xxx=com.xxx.XxxStatusChecker)
```

XxxStatusChecker.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.common.status.StatusChecker;
 
public class XxxStatusChecker implements StatusChecker {
    public Status check() {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.common.status.StatusChecker：

```properties
xxx=com.xxx.XxxStatusChecker
```
