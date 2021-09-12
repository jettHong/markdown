# Mapping路径源码分析

------

总体类继承图
重点：RequestMappingHandlerMapping

![QQ截图20210710191242](mapping.assets/QQ截图20210710191242.png)

RequestMappingHandlerMapping.afterPropertiesSet() 覆写父类 AbstractHandlerMethodMapping。

![image-20210710191122188](mapping.assets/image-20210710191122188.png)

![image-20210710190958195](mapping.assets/image-20210710190958195.png)

```java
AbstractHandlerMethodMapping.processCandidateBean();
```

![image-20210710190705356](mapping.assets/image-20210710190705356.png)

```java
AbstractHandlerMethodMapping.processCandidateBean();
AbstractHandlerMethodMapping.detectHandlerMethods(Object handler);
```

![image-20210710190343232](mapping.assets/image-20210710190343232.png)



```java
RequestMappingHandlerMapping.getMappingForMethod(Method method, Class<?> handlerType);
```

整合类路径与方法路径
如下图 类路径 = handlerType->typeInfo，方法路径 = method->info

![image-20210710185252998](mapping.assets/image-20210710185252998.png)

![image-20210710185606595](mapping.assets/image-20210710185606595.png)





------

下面暂时无用

![image-20210708152903203](mapping.assets/image-20210708152903203.png)

```java
// 入口
protected void doDispatch(HttpServletRequest request, HttpServletResponse response) throws Exception

// 取得处理类 
protected HandlerExecutionChain getHandler(HttpServletRequest request) throws Exception {
    if (this.handlerMappings != null) {
        for (HandlerMapping hm : this.handlerMappings) {
            if (logger.isTraceEnabled()) {
                logger.trace(
                    "Testing handler map [" + hm + "] in DispatcherServlet with name '" + getServletName() + "'");
            }
            HandlerExecutionChain handler = hm.getHandler(request);
            if (handler != null) {
                return handler; // 当前是 RequestMappingHandlerMapping 这个类取得
            }
        }
    }
    return null;
}
```

![image-20210708220051510](mapping.assets/image-20210708220051510.png)