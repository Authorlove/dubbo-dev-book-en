# cluster contension

## description

when there are multiple service providers, you can organize these providers into a cluster and take it as one provider.

## interface

`com.alibaba.dubbo.rpc.cluster.Cluster`

## configuration

```xml
<dubbo:protocol cluster="xxx" />
<!-- default setting for <dubbo:protocol> -->
<dubbo:provider cluster="xxx" />
```

## existing extensions

* `com.alibaba.dubbo.rpc.cluster.support.FailoverCluster`
* `com.alibaba.dubbo.rpc.cluster.support.FailfastCluster`
* `com.alibaba.dubbo.rpc.cluster.support.FailsafeCluster`
* `com.alibaba.dubbo.rpc.cluster.support.FailbackCluster`
* `com.alibaba.dubbo.rpc.cluster.support.ForkingCluster`
* `com.alibaba.dubbo.rpc.cluster.support.AvailableCluster`

## examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxCluster.java (implements Cluster)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.cluster.Cluster (plain text file with content of xxx=com.xxx.XxxCluster)
```

XxxCluster.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.cluster.Cluster;
import com.alibaba.dubbo.rpc.cluster.support.AbstractClusterInvoker;
import com.alibaba.dubbo.rpc.cluster.Directory;
import com.alibaba.dubbo.rpc.cluster.LoadBalance;
import com.alibaba.dubbo.rpc.Invoker;
import com.alibaba.dubbo.rpc.Invocation;
import com.alibaba.dubbo.rpc.Result;
import com.alibaba.dubbo.rpc.RpcException;
 
public class XxxCluster implements Cluster {
    public <T> Invoker<T> merge(Directory<T> directory) throws RpcException {
        return new AbstractClusterInvoker<T>(directory) {
            public Result doInvoke(Invocation invocation, List<Invoker<T>> invokers, LoadBalance loadbalance) throws RpcException {
                // ...
            }
        };
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.cluster.Cluster：

```properties
xxx=com.xxx.XxxCluster
```
