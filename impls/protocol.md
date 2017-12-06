# Protocol extension

## Instructions

RPC protocol extension is used to hide the details of remote call

Agreement:

* Invoker object is returned when invoke `Protocol#refer` and when user invokes `Invoker#invoke`，the protocol should promise that `Invoker#invoke` is executed and the `Invoker` reference is the one that passed to  `Protocol#export()`.
* Among them, the `Invoker` returned by `refer ()` is implemented by protocol. Protocols usually need to send remote requests in this `Invoker`. `export ()'s incoming `Invoker` is implemented and imported by frame, and protocol needs no concern.

Be careful：

* the protocol does not care about the transparent agent of the business interface, centered on the `Invoker`, and transforms the `Invoker` into a business interface from the outer layer.
* the protocol is not necessarily a TCP network communication, such as through shared files, IPC interprocess communications, etc.

## Interface

* `com.alibaba.dubbo.rpc.Protocol`
* `com.alibaba.dubbo.rpc.Exporter`
* `com.alibaba.dubbo.rpc.Invoker`

```java
public interface Protocol {
    /**
     * Exposing remote service: <br>
     * 1. When a request is received, the protocol should record the source address information of the request: RpcContext.getContext().setRemoteAddress();<br>
     * 2. Export() must be idempotent, that is, to expose the same URL Invoker two times, and not to be different from exposure. <br>
     * 3. The export() incoming Invoker is implemented and passed in by the framework, and the protocol needs no concern.<br>
     * 
     * @param <T> Type of service
     * @param invoker Service executor
     * @return exporter A reference to an exposure service, used to cancel exposure
     * @throws RpcException Thrown out when the exposure service is wrong, such as the port has been occupied
     */
    <T> Exporter<T> export(Invoker<T> invoker) throws RpcException;
 
    /**
     * referencing remote service: <br>
     * 1. when the user calls the invoke () method of the Invoker object returned by refer (), the protocol needs to execute the invoke () method of Invoker object that is introduced into the URL remote export (). <br>
     * 2. The Invoker returned by refer () is implemented by the protocol, and the protocol usually needs to send a remote request in this Invoker. <br>
     * 3. When the check=false is set in URL, the connection failure can not throw out the exception, and the internal recovery is required. <br>
     * 
     * @param <T> type of service
     * @param type type of service
     * @param url The URL address of a remote service
     * @return invoker Local agent for service
     * @throws RpcException Throw out when the connection service provider fails
     */
    <T> Invoker<T> refer(Class<T> type, URL url) throws RpcException;
 
}
```

## Configuration

```xml
<!-- If Id is not configured, name will be used as ID -->
<dubbo:protocol id="xxx1" name="xxx" />
<!-- if the protocol property is not configured, the protocol configuration will be automatically scanned in the ApplicationContext -->
<dubbo:service protocol="xxx1" />
<!-- using this configuration when the <dubbo:service> does not configure the prototol property -->
<dubbo:provider protocol="xxx1" />
```

## Known contension

* `com.alibaba.dubbo.rpc.injvm.InjvmProtocol`
* `com.alibaba.dubbo.rpc.dubbo.DubboProtocol`
* `com.alibaba.dubbo.rpc.rmi.RmiProtocol`
* `com.alibaba.dubbo.rpc.http.HttpProtocol`
* `com.alibaba.dubbo.rpc.http.hessian.HessianProtocol`

## Demo

Maven project structure：

```

src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxProtocol.java (implements Protocol)
                |-XxxExporter.java (implements Exporter)
                |-XxxInvoker.java (implements Invoker)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.rpc.Protocol (Plain text file, content：xxx=com.xxx.XxxProtocol)
```

XxxProtocol.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.Protocol;
 
public class XxxProtocol implements Protocol {
    public <T> Exporter<T> export(Invoker<T> invoker) throws RpcException {
        return new XxxExporter(invoker);
    }
    public <T> Invoker<T> refer(Class<T> type, URL url) throws RpcException {
        return new XxxInvoker(type, url);
    }
}
```

XxxExporter.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.support.AbstractExporter;
 
public class XxxExporter<T> extends AbstractExporter<T> {
    public XxxExporter(Invoker<T> invoker) throws RemotingException{
        super(invoker);
        // ...
    }
    public void unexport() {
        super.unexport();
        // ...
    }
}
```

XxxInvoker.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.rpc.support.AbstractInvoker;
 
public class XxxInvoker<T> extends AbstractInvoker<T> {
    public XxxInvoker(Class<T> type, URL url) throws RemotingException{
        super(type, url);
    }
    protected abstract Object doInvoke(Invocation invocation) throws Throwable {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.rpc.Protocol：

```properties
xxx=com.xxx.XxxProtocol
```