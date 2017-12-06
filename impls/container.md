# Container extension

## Description

Extension for server container.

## Interface

`com.alibaba.dubbo.container.Container`

## Configuration

```sh
java com.alibaba.dubbo.container.Main spring jetty log4j
```

## Existing extension

* `com.alibaba.dubbo.container.spring.SpringContainer`
* `com.alibaba.dubbo.container.spring.JettyContainer`
* `com.alibaba.dubbo.container.spring.Log4jContainer`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxContainer.java (implements Container)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.container.Container (Plain text file with content of xxx=com.xxx.XxxContainer)
```

XxxContainer.java：

```java
package com.xxx;
 
com.alibaba.dubbo.container.Container;
 
 
public class XxxContainer implements Container {
    public Status start() {
        // ...
    }
    public Status stop() {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.container.Container：

```properties
xxx=com.xxx.XxxContainer
```
