# Network node grouping extension

## Description

Peer to peer network node grouping extension.

## Interface

`com.alibaba.dubbo.remoting.p2p.Networker`

## Configuration

```xml
<dubbo:protocol networker="xxx" />
<!-- Default setting, when <dubbo:protocol> does not configure the networker property, use this configuration -->
<dubbo:provider networker="xxx" /> 
```

## Known extension

* `com.alibaba.dubbo.remoting.p2p.support.MulticastNetworker`
* `com.alibaba.dubbo.remoting.p2p.support.FileNetworker`

## Example

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxNetworker.java (implement Networker)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.remoting.p2p.Networker (Plain text file，content：xxx=com.xxx.XxxNetworker)
```

XxxNetworker.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.remoting.p2p.Networker;
 
public class XxxNetworker implements Networker {
    public Group lookup(URL url) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.remoting.p2p.Networker：

```properties
xxx=com.xxx.XxxNetworker
```
