# Exporter listener contension

## Description

When exporting service, notify this listener.

## Interface

`com.alibaba.dubbo.rpc.ExporterListener`

## Configuration

```xml
<!-- listener for exporting service  -->
<dubbo:service listener="xxx,yyy" />
<!-- default listener for exporting service -->
<dubbo:provider listener="xxx,yyy" />
```

## Known contension

`com.alibaba.dubbo.registry.directory.RegistryExporterListener`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxExporterListener.java (implements ExporterListener)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.ExporterListener (Plain context file with content of xxx=com.xxx.XxxExporterListener)
```

XxxExporterListener.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.ExporterListener;
import com.alibaba.dubbo.rpc.Exporter;
import com.alibaba.dubbo.rpc.RpcException;
 
 
public class XxxExporterListener implements ExporterListener {
    public void exported(Exporter<?> exporter) throws RpcException {
        // ...
    }
    public void unexported(Exporter<?> exporter) throws RpcException {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.ExporterListener：

```properties
xxx=com.xxx.XxxExporterListener
```

