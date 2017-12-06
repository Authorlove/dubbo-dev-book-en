# Compiler extension

## Description

Java code comipler for dynamically generating byte code in order to accelerate the process of invocation.

## Interface

`com.alibaba.dubbo.common.compiler.Compiler`

## Configuration

autoloaded

## Existing extensions

* `com.alibaba.dubbo.common.compiler.support.JdkCompiler`
* `com.alibaba.dubbo.common.compiler.support.JavassistCompiler`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxCompiler.java (implements Compiler)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.common.compiler.Compiler (plain text file with content of：xxx=com.xxx.XxxCompiler)
```

XxxCompiler.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.common.compiler.Compiler;
 
public class XxxCompiler implements Compiler {
    public Object getExtension(Class<?> type, String name) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.common.compiler.Compiler：

```properties
xxx=com.xxx.XxxCompiler
```
