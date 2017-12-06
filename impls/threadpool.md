# Thread pool contension

## Description

The service provider's thread pool strategy: when the server recieves a request, it needs allocate a thread from the thread pool to execute the service's method.

## Interface

`com.alibaba.dubbo.common.threadpool.ThreadPool`

## Configuration

```xml
<dubbo:protocol threadpool="xxx" />
<!-- low priority -->
<dubbo:provider threadpool="xxx" />
```

## Known extension

* `com.alibaba.dubbo.common.threadpool.FixedThreadPool`
* `com.alibaba.dubbo.common.threadpool.CachedThreadPool`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxThreadPool.java (implements ThreadPool)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.common.threadpool.ThreadPool (plain text file with content of xxx=com.xxx.XxxThreadPool)
```

XxxThreadPool.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.common.threadpool.ThreadPool;
import java.util.concurrent.Executor;
 
public class XxxThreadPool implements ThreadPool {
    public Executor getExecutor() {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.common.threadpool.ThreadPool：

```properties
xxx=com.xxx.XxxThreadPool
```

