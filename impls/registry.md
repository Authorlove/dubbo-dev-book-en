# Registry contension

## Description

Registry is responsible for the registration and discovery of the service.

## Interface

* `com.alibaba.dubbo.registry.RegistryFactory`
* `com.alibaba.dubbo.registry.Registry`

## Configuration

```xml
<!-- define registry -->
<dubbo:registry id="xxx1" address="xxx://ip:port" />
<!-- if the registry property is not configured, the registry configuration will be automatically scanned in the ApplicationContext -->
<dubbo:service registry="xxx1" />
<!-- When <dubbo:service> does not configure the registry property, use this configuration -->
<dubbo:provider registry="xxx1" />
```

## Agreement

RegistryFactory.java：

```java
public interface RegistryFactory {
    /**
     * registry needs follow contract：<br>
     * 1. it does not check the connection when setting up check=false, otherwise the exception is thrown when the connection failed to create. <br>
     * 2. supports username:password to configure authentication on URL. <br>
     * 3. supports backup=10.20.153.10 to configure candidate registry cluster address.<br>
     * 4. supports the file=registry.cache to configure local disk file cache. <br>
     * 5. supports the timeout=1000 for request timeout setting. <br>
     * 6. supports session=60000 for session timeout or expiration settings. <br>
     * 
     * @param url registry address，not allowed to be empty
     * @return registry reference，not null
     */
    Registry getRegistry(URL url); 
}
```

RegistryService.java：

```java
public interface RegistryService { // Registry extends RegistryService 
    /**
     * registration service.
     * 
     * registration needs to deal with the contract: <br>
     * 1. when the URL has set up the check=false, the registration fails, and retries in the background, otherwise the exception is thrown. <br>
     * 2. when the dynamic=false parameter is set in URL, it needs to be stored persistently, otherwise, it should be deleted automatically when the registrant has an abnormal exit. <br>
     * 3. when the URL is set up category=overrides, it represents the classified storage, the default category is providers, and the data can be notified by the classified section. <br>
     * 4. when the registry is restarted, network jitter, and no data can be lost, including automatically deleting data from the broken line. <br>
     * 5. allows the coexistence of URL with the same URI but different parameters, and can not be covered. <br>
     * 
     * @param url registration information, which is not allowed to be empty，如：dubbo://10.20.153.10/com.alibaba.foo.BarService?version=1.0.0&application=kylin
     */
    void register(URL url);
 
    /**
     * Cancel the registration service.
     * 
     * cancelling the registration needs to deal with the contract: <br>
     * 1. if it is the persistent stored data of the dynamic=false, the registration data can not be found, then the * *IllegalStateException is thrown, otherwise it is ignored. <br>
     * 2. cancels the registration according to the full URL match. <br>
     * 
     * @param url Registration information is not allowed to be empty, such as,dubbo://10.20.153.10/com.alibaba.foo.BarService?version=1.0.0&application=kylin
     */
    void unregister(URL url);
 
    /**
     * subscribe service.
     * 
     * subscribing service needs to deal with the contract：<br>
     * 1. when the URL is set up, the subscription fails and is retried in the background. <br>
     * 2. when URL sets category=overrides, it only notifies the specified classification data. Multiple classifications
     * are separated by commas, and allows asterisk to match, which indicates that all categorical data are subscribed. <br>
     * 3. allows interface, group, version, and classifier as a conditional query, such as   
     * interface=com.alibaba.foo.BarService&version=1.0.0<br>
     * 4. and the query conditions allow the stars to match, subscribe to all versions of all the packets of all interfaces,* or: interface=*&group=*&version=*&classifier=*<br>
     * 5. automatically restore the subscription request when the registry is restarted and the network jitter. <br>
     * 6. allows the coexistence of URL with the same URI but different parameters, and can not be covered. <br>
     * 7. must block the subscription process and return after the first notice. <br>
     * 
     * @param url Subscribe condition, not allowed to be empty, such as consumer://10.20.153.10/com.alibaba.foo.BarService? * version=1.0.0&application=kylin
     * @param listener Change the event listener, not allowed to be empty
     */
    void subscribe(URL url, NotifyListener listener);
 
    /**
     * unsubscribe service.
     * 
     * cancelling a subscription to deal with the contract: <br>
     * 1. if not subscribed, directly ignored. <br>
     * 2. unsubscribe by full URL match. <br>
     * 
     * @param url subscription condition, which is not allowed to be empty, such as:   
     * consumer://10.20.153.10/com.alibaba.foo.BarService?version=1.0.0&application=kylin
     * @param listener change event listener, not allowed to be empty
     */
    void unsubscribe(URL url, NotifyListener listener);
 
    /**
     * Query the registration list, corresponding to the push mode of the subscription, here is the pull mode and returns *only one result.
     * 
     * @see com.alibaba.dubbo.registry.NotifyListener#notify(List)
     * @param url query condition, not allowed to be empty, such as: consumer://10.20.153.10/com.alibaba.foo.BarService?version=1.0.0&application=kylin
     * @return The registered information list, which may be empty, means the parameters of the same 
     * {@link com.alibaba.dubbo.registry.NotifyListener#notify (List<URL>)}.
     */
    List<URL> lookup(URL url);
 
}
```

NotifyListener.java：

```java
public interface NotifyListener { 
    /**
     * triggers when a service change notification is received.
     * 
     * notification needs to deal with the contract: <br>
     * 1. always uses the service interface and data type as the dimension total notification, that is, it will not tell the same type of data of a service. Users do not need to compare the result of the previous notification. <br>
     * 2. the first notice at 2. subscriptions must be full notification of all types of data of a service. <br>
     * 3. allowing different types of data to be notified separately when changing, such as providers, consumers, routes and overrides, allowing only one type to be notified, but the data of this type must be full, not incremental. <br>
     * 4. if one type of data is empty, a empty protocol and an identifier URL data with a category parameter are required to be notified. <br>
     * 5. notifications (i.e., the registry Implementation) need to ensure the order of notifications, such as single thread push, queue serialization, and version comparison. <br>
     * 
     * @param urls The registered information list is always not empty, and the meaning is the same as the return value of  * {@link com.alibaba.dubbo.registry.RegistryService#lookup (URL)}.
     */
    void notify(List<URL> urls);
 
}
```

## Contension

`com.alibaba.dubbo.registry.support.dubbo.DubboRegistryFactory`

## Example

Maven project structure:

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxRegistryFactoryjava (implements RegistryFactory)
                |-XxxRegistry.java (implements Registry)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.registry.RegistryFactory (plain text file, content：xxx=com.xxx.XxxRegistryFactory)
```

XxxRegistryFactory.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.registry.RegistryFactory;
import com.alibaba.dubbo.registry.Registry;
import com.alibaba.dubbo.common.URL;
 
public class XxxRegistryFactory implements RegistryFactory {
    public Registry getRegistry(URL url) {
        return new XxxRegistry(url);
    }
}
```

XxxRegistry.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.registry.Registry;
import com.alibaba.dubbo.registry.NotifyListener;
import com.alibaba.dubbo.common.URL;
 
public class XxxRegistry implements Registry {
    public void register(URL url) {
        // ...
    }
    public void unregister(URL url) {
        // ...
    }
    public void subscribe(URL url, NotifyListener listener) {
        // ...
    }
    public void unsubscribe(URL url, NotifyListener listener) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.registry.RegistryFactory：

```properties
xxx=com.xxx.XxxRegistryFactory
```