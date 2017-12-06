# Dynamic proxy extension

## Description

Convert the `Invoker` interface into a business interface.

## Interface

`com.alibaba.dubbo.rpc.ProxyFactory`

## Configuration

```xml
<dubbo:protocol proxy="xxx" />
<!-- Default configuration, when <dubbo:protocol> does not configure the proxy property, use this configuration -->
<dubbo:provider proxy="xxx" />
```

## Known extension

* `com.alibaba.dubbo.rpc.proxy.JdkProxyFactory`
* `com.alibaba.dubbo.rpc.proxy.JavassistProxyFactory`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxProxyFactory.java (实现ProxyFactory接口)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.ProxyFactory (纯文本文件，内容为：xxx=com.xxx.XxxProxyFactory)
```

XxxProxyFactory.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.ProxyFactory;
import com.alibaba.dubbo.rpc.Invoker;
import com.alibaba.dubbo.rpc.RpcException;
 
 
public class XxxProxyFactory implements ProxyFactory {
    public <T> T getProxy(Invoker<T> invoker) throws RpcException {
        // ...
    }
    public <T> Invoker<T> getInvoker(T proxy, Class<T> type, URL url) throws RpcException {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.ProxyFactory：

```properties
xxx=com.xxx.XxxProxyFactory
```
