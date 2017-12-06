# Router extension

## Description

Choose one of the multiple service providers to make a call.

## Interface

* `com.alibaba.dubbo.rpc.cluster.RouterFactory`
* `com.alibaba.dubbo.rpc.cluster.Router`

## Configuration

```xml
<dubbo:protocol router="xxx" />
<!-- Default setting, when <dubbo:protocol> does not configure loadbalance, use this configuration -->
<dubbo:provider router="xxx" />
```

## Known extension

* `com.alibaba.dubbo.rpc.cluster.router.ScriptRouterFactory`
* `com.alibaba.dubbo.rpc.cluster.router.FileRouterFactory`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxRouterFactory.java (implements LoadBalance)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.cluster.RouterFactory (pure text file，content：xxx=com.xxx.XxxRouterFactory)

```

XxxRouterFactory.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.cluster.RouterFactory;
import com.alibaba.dubbo.rpc.Invoker;
import com.alibaba.dubbo.rpc.Invocation;
import com.alibaba.dubbo.rpc.RpcException;
 
public class XxxRouterFactory implements RouterFactory {
    public <T> List<Invoker<T>> select(List<Invoker<T>> invokers, Invocation invocation) throws RpcException {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.cluster.RouterFactory：

```properties
xxx=com.xxx.XxxRouterFactory
```
