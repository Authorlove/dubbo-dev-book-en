# Network transmission extension

## Description

Implementation of remote communication between server and client.

## Interface

* `com.alibaba.dubbo.remoting.Transporter`
* `com.alibaba.dubbo.remoting.Server`
* `com.alibaba.dubbo.remoting.Client`

## Configuration

```xml
<!-- server and client use the same implementation -->
<dubbo:protocol transporter="xxx" /> 
<!-- server and client use different implementation -->
<dubbo:protocol server="xxx" client="xxx" /> 
<!-- Default setting, when <dubbo:protocol> does not configure the transporter/server/client property, use this configuration -->
<dubbo:provider transporter="xxx" server="xxx" client="xxx" />
```

## Known configuration

* `com.alibaba.dubbo.remoting.transport.transporter.netty.NettyTransporter`
* `com.alibaba.dubbo.remoting.transport.transporter.mina.MinaTransporter`
* `com.alibaba.dubbo.remoting.transport.transporter.grizzly.GrizzlyTransporter`

## Examples

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxTransporter.java (implments Transporter)
                |-XxxServer.java (implements Server)
                |-XxxClient.java (implements Client)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.remoting.Transporter (pure text file，content：xxx=com.xxx.XxxTransporter)
```

XxxTransporter.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.remoting.Transporter;
 
public class XxxTransporter implements Transporter {
    public Server bind(URL url, ChannelHandler handler) throws RemotingException {
        return new XxxServer(url, handler);
    }
    public Client connect(URL url, ChannelHandler handler) throws RemotingException {
        return new XxxClient(url, handler);
    }
}
```

XxxServer.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.remoting.transport.transporter.AbstractServer;
 
public class XxxServer extends AbstractServer {
    public XxxServer(URL url, ChannelHandler handler) throws RemotingException{
        super(url, handler);
    }
    protected void doOpen() throws Throwable {
        // ...
    }
    protected void doClose() throws Throwable {
        // ...
    }
    public Collection<Channel> getChannels() {
        // ...
    }
    public Channel getChannel(InetSocketAddress remoteAddress) {
        // ...
    }
}
```

XxxClient.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.remoting.transport.transporter.AbstractClient;
 
public class XxxClient extends AbstractClient {
    public XxxServer(URL url, ChannelHandler handler) throws RemotingException{
        super(url, handler);
    }
    protected void doOpen() throws Throwable {
        // ...
    }
    protected void doClose() throws Throwable {
        // ...
    }
    protected void doConnect() throws Throwable {
        // ...
    }
    public Channel getChannel() {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.remoting.Transporter：

```properties
xxx=com.xxx.XxxTransporter
```
