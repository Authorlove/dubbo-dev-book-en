# Loader extension

## Description

Loader extension to specify containers to load extension

## Interface

`com.alibaba.dubbo.common.extension.ExtensionFactory`

## Configuration

```xml
<dubbo:application compiler="jdk" />
```

## Known extensions

* `com.alibaba.dubbo.common.extension.factory.SpiExtensionFactory`
* `com.alibaba.dubbo.config.spring.extension.SpringExtensionFactory`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxExtensionFactory.java (implements ExtensionFactory)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.common.extension.ExtensionFactory (Plain context file with content of xxx=com.xxx.XxxExporterListenerxxx=com.xxx.XxxExtensionFactory)
```

XxxExtensionFactory.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.common.extension.ExtensionFactory;
 
public class XxxExtensionFactory implements ExtensionFactory {
    public Object getExtension(Class<?> type, String name) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.common.extension.ExtensionFactory：

```properties
xxx=com.xxx.XxxExtensionFactory
```
