# Page extension

## Description

Extension for rendering page of url.

## Interface

`com.alibaba.dubbo.container.page.PageHandler`

## Configuration

```xml
<dubbo:protocol page="xxx,yyy" />
<!-- Default setting, when <dubbo:protocol> does not configure the page property, use this configuration -->
<dubbo:provider page="xxx,yyy" />
```

## Known extension

* `com.alibaba.dubbo.container.page.pages.HomePageHandler`
* `com.alibaba.dubbo.container.page.pages.StatusPageHandler`
* `com.alibaba.dubbo.container.page.pages.LogPageHandler`
* `com.alibaba.dubbo.container.page.pages.SystemPageHandler`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxPageHandler.java (implement PageHandler)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.container.page.PageHandler (Plain text file，content：xxx=com.xxx.XxxPageHandler)
```

XxxPageHandler.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.container.page.PageHandler;
 
public class XxxPageHandler implements PageHandler {
    public Group lookup(URL url) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.container.page.PageHandler：

```properties
xxx=com.xxx.XxxPageHandler
```
