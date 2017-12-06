# Contension for merging result

## Description

Merge mulitiple request's responses

## Interface

`com.alibaba.dubbo.rpc.cluster.Merger`

## Configuration

```xml
<dubbo:method merger="xxx" />
```

## Known extensions

* `com.alibaba.dubbo.rpc.cluster.merger.ArrayMerger`
* `com.alibaba.dubbo.rpc.cluster.merger.ListMerger`
* `com.alibaba.dubbo.rpc.cluster.merger.SetMerger`
* `com.alibaba.dubbo.rpc.cluster.merger.MapMerger`

## Exampes

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxMerger.java (implements Merger)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.cluster.Merger (Plain text file with content of xxx=com.xxx.XxxMerger)
```

XxxMerger.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.cluster.Merger;
 
public class XxxMerger<T> implements Merger<T> {
    public T merge(T... results) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.cluster.Merger：

```properties
xxx=com.xxx.XxxMerger
```

