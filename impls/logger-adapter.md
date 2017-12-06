# Logger adaptor contension

## Description

Contension for adapting logger。

## Interface

`com.alibaba.dubbo.common.logger.LoggerAdapter`

## Configuration

```xml
<dubbo:application logger="xxx" />
```

or：

```sh
-Ddubbo:application.logger=xxx
```

## Known extensions

* `com.alibaba.dubbo.common.logger.slf4j.Slf4jLoggerAdapter`
* `com.alibaba.dubbo.common.logger.jcl.JclLoggerAdapter`
* `com.alibaba.dubbo.common.logger.log4j.Log4jLoggerAdapter`
* `com.alibaba.dubbo.common.logger.jdk.JdkLoggerAdapter`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxLoggerAdapter.java (implements LoggerAdapter)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.common.logger.LoggerAdapter (Plain text file with content of xxx=com.xxx.XxxLoggerAdapter)
```

XxxLoggerAdapter.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.common.logger.LoggerAdapter;
 
public class XxxLoggerAdapter implements LoggerAdapter {
    public Logger getLogger(URL url) {
        // ...
    }
}
```

XxxLogger.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.common.logger.Logger;
 
public class XxxLogger implements Logger {
    public XxxLogger(URL url) {
        // ...
    }
    public void info(String msg) {
        // ...
    }
    // ...
}
```

META-INF/dubbo/com.alibaba.dubbo.common.logger.LoggerAdapter：

```properties
xxx=com.xxx.XxxLoggerAdapter
```