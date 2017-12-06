# Validation contension

## Description

Argument validator contension.

## Interface

`com.alibaba.dubbo.validation.Validation`

## Configuration

```xml
<dubbo:service validation="xxx,yyy" />
<!-- lower priority -->
<dubbo:provider validation="xxx,yyy" />
```

## Known extension

`com.alibaba.dubbo.validation.support.jvalidation.JValidation`

## Examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxValidation.java (implement Validation)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.validation.Validation (text file with content of xxx=com.xxx.XxxValidation)
```

XxxValidation.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.validation.Validation;
 
public class XxxValidation implements Validation {
    public Object getValidator(URL url) {
        // ...
    }
}
```

XxxValidator.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.validation.Validator;
 
public class XxxValidator implements Validator {
    public XxxValidator(URL url) {
        // ...
    }
    public void validate(Invocation invocation) throws Exception {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.validation.Validation：

```properties
xxx=com.xxx.XxxValidation
```