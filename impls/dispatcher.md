# Message dispatcher extension

## Description

Channel's message dispather is used to specify the thread pool model which executes the message.

## Interface

`com.alibaba.dubbo.remoting.Dispatcher`

## Configuration

```xml
<dubbo:protocol dispatcher="xxx" />
<!-- when not configuring <dubbo:protocol> with dispatcher attribute，we use this configuration -->
<dubbo:provider dispatcher="xxx" />
```

## Existing extented implementations

* `com.alibaba.dubbo.remoting.transport.dispatcher.all.AllDispatcher`
* `com.alibaba.dubbo.remoting.transport.dispatcher.direct.DirectDispatcher`
* `com.alibaba.dubbo.remoting.transport.dispatcher.message.MessageOnlyDispatcher`
* `com.alibaba.dubbo.remoting.transport.dispatcher.execution.ExecutionDispatcher`
* `com.alibaba.dubbo.remoting.transport.dispatcher.connection.ConnectionOrderedDispatcher`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxDispatcher.java (implements Dispatcher)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.remoting.Dispatcher (plain text file with content of xxx=com.xxx.XxxDispatcher)
```

XxxDispatcher.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.remoting.Dispatcher;
 
public class XxxDispatcher implements Dispatcher {
    public Group lookup(URL url) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.remoting.Dispatcher：

```properties
xxx=com.xxx.XxxDispatcher
```
