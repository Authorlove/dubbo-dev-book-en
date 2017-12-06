# cache extension

## description

using requst's arguments as key to cache its response

## interface

`com.alibaba.dubbo.cache.CacheFactory`

## configuration

```xml
<dubbo:service cache="lru" />
<!-- method level cache -->
<dubbo:service><dubbo:method cache="lru" /></dubbo:service>
<!-- default setting for <dubbo:service> -->
<dubbo:provider cache="xxx,yyy" />
```

## exsiting extensions

* `com.alibaba.dubbo.cache.support.lru.LruCacheFactory`
* `com.alibaba.dubbo.cache.support.threadlocal.ThreadLocalCacheFactory`
* `com.alibaba.dubbo.cache.support.jcache.JCacheFactory`


## examples

Maven project struture：

```
src
 |-main
    |-java
        |-com
            |-xxx
                |-XxxCacheFactory.java (implements StatusChecker)
    |-resources
        |-META-INF
            |-dubbo
                |-com.alibaba.dubbo.cache.CacheFactory (a plain text file with content of xxx=com.xxx.XxxCacheFactory)
```

XxxCacheFactory.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.cache.CacheFactory;
 
public class XxxCacheFactory implements CacheFactory {
    public Cache getCache(URL url, String name) {
        return new XxxCache(url, name);
    }
}
```

XxxCacheFactory.java：

```java
package com.xxx;
 
import com.alibaba.dubbo.cache.Cache;
 
public class XxxCache implements Cache {
    public Cache(URL url, String name) {
        // ...
    }
    public void put(Object key, Object value) {
        // ...
    }
    public Object get(Object key) {
        // ...
    }
}
```

META-INF/dubbo/com.alibaba.dubbo.cache.CacheFactory：

```properties
xxx=com.xxx.XxxCacheFactory
```