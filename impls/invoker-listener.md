# Reference listener contension

## Description

Notify listeners when service is referred.

## Interface

`com.alibaba.dubbo.rpc.InvokerListener`

## Configuration

```xml
<!-- reference configuration override consumer's configuration -->
<dubbo:reference listener="xxx,yyy" /> 
<!-- consumer configuration -->
<dubbo:consumer listener="xxx,yyy" /> 
```

## Known extension

`com.alibaba.dubbo.rpc.listener.DeprecatedInvokerListener`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxInvokerListener.java (implements InvokerListener)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.InvokerListener (Plain text file with content of xxx=com.xxx.XxxInvokerListener)
```

XxxInvokerListener.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.InvokerListener;
import com.alibaba.dubbo.rpc.Invoker;
import com.alibaba.dubbo.rpc.RpcException;
 
public class XxxInvokerListener implements InvokerListener {
    public void referred(Invoker<?> invoker) throws RpcException {
        // ...
    }
    public void destroyed(Invoker<?> invoker) throws RpcException {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.InvokerListener：

```properties
xxx=com.xxx.XxxInvokerListener
```
