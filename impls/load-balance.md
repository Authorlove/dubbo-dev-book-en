# Load balancer contension

## Description

select one from multiple service providers to invoke

## Interface

`com.alibaba.dubbo.rpc.cluster.LoadBalance`

## Configuration

```xml
<dubbo:protocol loadbalance="xxx" />
<!-- lower priority -->
<dubbo:provider loadbalance="xxx" />
```

## Known extensions

* `com.alibaba.dubbo.rpc.cluster.loadbalance.RandomLoadBalance`
* `com.alibaba.dubbo.rpc.cluster.loadbalance.RoundRobinLoadBalance`
* `com.alibaba.dubbo.rpc.cluster.loadbalance.LeastActiveLoadBalance`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxLoadBalance.java (实现LoadBalance接口)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.cluster.LoadBalance (纯文本文件，内容为：xxx=com.xxx.XxxLoadBalance)
```

XxxLoadBalance.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.cluster.LoadBalance;
import com.alibaba.dubbo.rpc.Invoker;
import com.alibaba.dubbo.rpc.Invocation;
import com.alibaba.dubbo.rpc.RpcException; 
 
public class XxxLoadBalance implements LoadBalance {
    public <T> Invoker<T> select(List<Invoker<T>> invokers, Invocation invocation) throws RpcException {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.cluster.LoadBalance：

```properties
xxx=com.xxx.XxxLoadBalance
```
