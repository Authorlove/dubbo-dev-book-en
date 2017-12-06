# Telnet command contension

## Description

All servers support telnet access for manual intervention.

## Interface

`com.alibaba.dubbo.remoting.telnet.TelnetHandler`

## Configuration

```xml
<dubbo:protocol telnet="xxx,yyy" />
<!-- Default setting, when <dubbo:protocol> does not configure the telnet property, use this configuration -->
<dubbo:provider telnet="xxx,yyy" />
```

## Known extension

* `com.alibaba.dubbo.remoting.telnet.support.ClearTelnetHandler`
* `com.alibaba.dubbo.remoting.telnet.support.ExitTelnetHandler`
* `com.alibaba.dubbo.remoting.telnet.support.HelpTelnetHandler`
* `com.alibaba.dubbo.remoting.telnet.support.StatusTelnetHandler`
* `com.alibaba.dubbo.rpc.dubbo.telnet.ListTelnetHandler`
* `com.alibaba.dubbo.rpc.dubbo.telnet.ChangeTelnetHandler`
* `com.alibaba.dubbo.rpc.dubbo.telnet.CurrentTelnetHandler`
* `com.alibaba.dubbo.rpc.dubbo.telnet.InvokeTelnetHandler`
* `com.alibaba.dubbo.rpc.dubbo.telnet.TraceTelnetHandler`
* `com.alibaba.dubbo.rpc.dubbo.telnet.CountTelnetHandler`
* `com.alibaba.dubbo.rpc.dubbo.telnet.PortTelnetHandler`

## Extension examples

Maven project structure：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxTelnetHandler.java (实现TelnetHandler接口)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.remoting.telnet.TelnetHandler (纯文本文件，内容为：xxx=com.xxx.XxxTelnetHandler)
```

XxxTelnetHandler.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.remoting.telnet.TelnetHandler;
 
@Help(parameter="...", summary="...", detail="...")
 
public class XxxTelnetHandler implements TelnetHandler {
    public String telnet(Channel channel, String message) throws RemotingException {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.remoting.telnet.TelnetHandler：

```properties
xxx=com.xxx.XxxTelnetHandler
```

## usage

```sh
telnet 127.0.0.1 20880
dubbo> xxx args
```
