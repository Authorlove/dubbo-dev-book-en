# Serialization extension

## Description

The object is converted into a byte stream, used for network transmission, and the bytes are transferred to objects, which are used to restore the object after the byte stream data is received.

## Interface

* `com.alibaba.dubbo.common.serialize.Serialization`
* `com.alibaba.dubbo.common.serialize.ObjectInput`
* `com.alibaba.dubbo.common.serialize.ObjectOutput`

## Configuration

```xml
<!-- Serialization of protocol -->
<dubbo:protocol serialization="xxx" />
<!-- Default setting, when <dubbo:protocol> does not configure serialization, use this configuration -->
<dubbo:provider serialization="xxx" />
```

## Known extension

* `com.alibaba.dubbo.common.serialize.dubbo.DubboSerialization`
* `com.alibaba.dubbo.common.serialize.hessian.Hessian2Serialization`
* `com.alibaba.dubbo.common.serialize.java.JavaSerialization`
* `com.alibaba.dubbo.common.serialize.java.CompactedJavaSerialization`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxSerialization.java (implements Serialization)
                |-XxxObjectInput.java (implements ObjectInput)
                |-XxxObjectOutput.java (implements ObjectOutput)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.common.serialize.Serialization (text file，content：xxx=com.xxx.XxxSerialization)
```

XxxSerialization.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.common.serialize.Serialization;
import com.alibaba.dubbo.common.serialize.ObjectInput;
import com.alibaba.dubbo.common.serialize.ObjectOutput;
 
 
public class XxxSerialization implements Serialization {
    public ObjectOutput serialize(Parameters parameters, OutputStream output) throws IOException {
        return new XxxObjectOutput(output);
    }
    public ObjectInput deserialize(Parameters parameters, InputStream input) throws IOException {
        return new XxxObjectInput(input);
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.common.serialize.Serialization：

```properties
xxx=com.xxx.XxxSerialization
```
