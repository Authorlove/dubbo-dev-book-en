# Invocation filter contension

## Description

Contension is for intercepting the invocation process of both service providers and service consumers, and most of Dubbo's own functionalities are based on this extension. The interceptors will be executed when a remote method is executed, so you should pay attention to its impact on performance.

Agreement：

* By default, custom filters are invoked after those which are built in DUBBO
* Specific value `default` represents the default filters, and you can use it to specify custom filter's order. For example,: `filter="xxx,default,yyy"`，represents `xxx` is in front of default filer and `yyy` is behind default filter。
* Specific symbol `-` means excluding filters, such as：`filter="-foo1"` represents excluding filter named `foo1` and `filter="-default"` represents excluding all of the default filters.
* when configuring filter for provider label and service label at the same time, it adds up all of the filters rather than overriding the previous, for example：configuration that `<dubbo:provider filter="xxx,yyy"/>` and `<dubbo:service filter="aaa,bbb" />` means `xxx`,`yyy`,`aaa` and `bbb` will take effect. If you want to override the previous settings, you should use this configuration：`<dubbo:service filter="-xxx,-yyy,aaa,bbb" />`

## Interface

`com.alibaba.dubbo.rpc.Filter`

## Configration

```xml
<!-- filters for a specific reference of the consumer-->
<dubbo:reference filter="xxx,yyy" />
<!-- default filters for all the references of the consumer -->
<dubbo:consumer filter="xxx,yyy"/>
<!-- filters for a specific service of the provider-->
<dubbo:service filter="xxx,yyy" />
<!-- default filters for all the services of the provider -->
<dubbo:provider filter="xxx,yyy"/>
```

## Existing extensions

* `com.alibaba.dubbo.rpc.filter.EchoFilter`
* `com.alibaba.dubbo.rpc.filter.GenericFilter`
* `com.alibaba.dubbo.rpc.filter.GenericImplFilter`
* `com.alibaba.dubbo.rpc.filter.TokenFilter`
* `com.alibaba.dubbo.rpc.filter.AccessLogFilter`
* `com.alibaba.dubbo.rpc.filter.CountFilter`
* `com.alibaba.dubbo.rpc.filter.ActiveLimitFilter`
* `com.alibaba.dubbo.rpc.filter.ClassLoaderFilter`
* `com.alibaba.dubbo.rpc.filter.ContextFilter`
* `com.alibaba.dubbo.rpc.filter.ConsumerContextFilter`
* `com.alibaba.dubbo.rpc.filter.ExceptionFilter`
* `com.alibaba.dubbo.rpc.filter.ExecuteLimitFilter`
* `com.alibaba.dubbo.rpc.filter.DeprecatedFilter`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxFilter.java (implements Filter)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.Filter (plain text file with content of xxx=com.xxx.XxxFilter)
```

XxxFilter.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.Filter;
import com.alibaba.dubbo.rpc.Invoker;
import com.alibaba.dubbo.rpc.Invocation;
import com.alibaba.dubbo.rpc.Result;
import com.alibaba.dubbo.rpc.RpcException;
 
public class XxxFilter implements Filter {
    public Result invoke(Invoker<?> invoker, Invocation invocation) throws RpcException {
        // before filter ...
        Result result = invoker.invoke(invocation);
        // after filter ...
        return result;
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.Filter：

```properties
xxx=com.xxx.XxxFilter
```