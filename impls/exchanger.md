# Message exchanger extension

## Description

Based on the transport layer, to achieve the Request-Response information exchange semantics.

## Interface

* `com.alibaba.dubbo.remoting.exchange.Exchanger`
* `com.alibaba.dubbo.remoting.exchange.ExchangeServer`
* `com.alibaba.dubbo.remoting.exchange.ExchangeClient`

## Configuration

```xml
<dubbo:protocol exchanger="xxx" />
<!-- Default configuration when <dubbo:protocol> doesn't have exchanger attribute -->
<dubbo:provider exchanger="xxx" />
```

## Known extensions

`com.alibaba.dubbo.remoting.exchange.exchanger.HeaderExchanger`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxExchanger.java (implements Exchanger)
                |-XxxExchangeServer.java (implements ExchangeServer)
                |-XxxExchangeClient.java (implements ExchangeClient)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.remoting.exchange.Exchanger (plain text file with content of xxx=com.xxx.XxxExchanger)
```

XxxExchanger.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.remoting.exchange.Exchanger;
 
 
public class XxxExchanger implements Exchanger {
    public ExchangeServer bind(URL url, ExchangeHandler handler) throws RemotingException {
        return new XxxExchangeServer(url, handler);
    }
    public ExchangeClient connect(URL url, ExchangeHandler handler) throws RemotingException {
        return new XxxExchangeClient(url, handler);
    }
}
```

XxxExchangeServer.java：

```java

package com.xxx;
 
import com.alibaba.dubbo.remoting.exchange.ExchangeServer;
 
public class XxxExchangeServer impelements ExchangeServer {
    // ...
}
```

XxxExchangeClient.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.remoting.exchange.ExchangeClient;
 
public class XxxExchangeClient impelments ExchangeClient {
    // ...
}
```

META-INF/dubbo/com.alibaba.dubbo.remoting.exchange.Exchanger：

```properties
xxx=com.xxx.XxxExchanger
```
