---
categories: 源码阅读
tags:
  - 分布式
sticky: ''
description: ''
permalink: ''
title: Nacos
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/d628cfe5-f52a-4092-bdc9-3b45d5c596ba/121397145_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R7FARLRT%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T180049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJGMEQCIBeDmklNDxZ8%2FFOUTo%2Bcj5%2F9cVnb460A4OqQxp64GkiDAiBVJKgDnyW%2BuWRp673DxXJSCyfNRU3t0ul%2FYREba3GODiqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM%2BsoWh%2FsHzSUuXvnlKtwD8UqnWO7rDqJHwhBXoRkpByDxiwa%2BiyxOelSqyEws%2F7vN%2BZC6rIZA9Za7hIFK5oQmDn0Pua%2FGkzKZN3wRl6y9LPoZ%2Foqk9Clh2TvjkDlTQChX%2FmD3FauyFzHz%2F3OcaITcX9gm%2BMruVi1Rc7WLg1REmcGrqqQGuojr9CWm%2BJJlJTrvHkOXzZvkd9HzslcJItYLnqtV3iLdb%2BwMoHwYQauIdB9x4QlyIBOmyqfO9QumKnS6mAqHg2Voeo6JGwmlGFm4KVOHCcts778wTWh6RlXAMVDdkI4HvOH%2BEzdC7QBvY3Q7i1sT9dbOySPkBIWYFAPWXKs%2BiXLGlko32o65XIL6uNDUbq7KlRtDeFUFQXPxy96OXE87ujVCOxZs8tNrJkCUkkO6mB8Hr6A8MyGbk0M%2FxQrkSkssG2ul4Mx4s4DTKmrkqqLqjE4pbiVt0%2Fj25R4MzPvcSSMg6KgOAiyoisoow7Qauo0xQ5vT900Zxv5vwof%2BB1Q41w21xQQuQMHsL%2F8kit%2FWFva7wQ9jNJlmfP35v9zOKlNX5%2F9mFit5yZS9CaOEV1cm1NvqajHOXF%2BlrF3MAHhy0%2BXFvzDdiL%2F5Y7sFOJe1Yf0O9LOmHcd%2Fg5TPsG5fLIODincD3TEIBKswu8u6xgY6pgHLcVlkAOyKy5YIY0Xwt4f6obIUvwG3I8JjDuuodb%2FNvR8LM7AJUK2A2sdPtPau37kLryBGocp1i2vb8kjg5mwmY9h6fBk38IFfpdbHIiJtAap4eG155iqCF%2FF%2FK00fIeT2kVMlu7aPb%2FxOcrFQ%2BQhZwm1jL8QAGn%2FY%2BKTtmKYFCtirhET5JjNp8Q9VMIxps5vc8EMFtHGYPoSn6H8hTy9hivWFmZfq&X-Amz-Signature=a7b271bc2305822af7df53195c21e71dbe67af62fca0535e1d3c6ff9761fb4de&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:58:00'
index_img: /images/27339a0cf59251c58d774f97963db81b.jpg
banner_img: /images/27339a0cf59251c58d774f97963db81b.jpg
---

架构图


![images0f6d00e5fda20c8cd454ccc3946a73c6.svg](/images/93576a4c7a5e815846105be6a1bb9b3f.svg)


三元组：namespace group service/dataid


# nacos数据交互模型


push（推模式）和pull（拉模式）


推模式：客户端和服务端建立好网络长链接，服务方有相关数据，直接通过长连接管道推送到客户端


优点：**及时**


缺点：不知道客户端的消费能力，可能导致数据积压在客户端


拉模式：客户端主动发起请求，拉相关数据


优点：不存在推模式的数据积压问题


缺点：不够及时


## 长轮询

> “轮询”是指不管服务端数据有无更新，客户端每隔定长时间请求拉取一次数据，可能有更新数据返回，也可能什么都没有。配置中心如果使用「轮询」实现动态推送，会有以下问题：
> - 推送延迟。客户端每隔 5s 拉取一次配置，若配置变更发生在第 6s，则配置推送的延迟会达到 4s。
> - 服务端压力。配置一般不会发生变化，频繁的轮询会给服务端造成很大的压力。
> - 推送延迟和服务端压力无法中和。降低轮询的间隔，延迟降低，压力增加；增加轮询的间隔，压力降低，延迟增高。
>

客户端发起长轮询，如果服务端的数据没有发生变更，会hold住这个请求，直接服务端的数据发生变化，或者等待一段时间超时之后才返回。返回之后，客户端又会立刻再次发起下一次长轮询。

- 推送延迟。服务端数据发生变更后，长轮询结束，立刻返回响应给客户端。
- 服务端压力。长轮询的间隔期一般很长，例如 30s、60s，并且服务端 hold 住连接不会消耗太多服务端资源。

![imagesccf4f1d60e29f9d8f72bcfd17b433b3e.png](/images/b0df3cb9f7f9b69074b84633aa787d8d.png)


**为什么长轮询要有超时时间：**

1. 稳定性：本质是http，会有服务端假死，fullgc等异常，**无心跳检测机制**
2. 业务性：用户可能会随时增加新的配置监听。而在此之前长轮询可能已经发出，无法监听新配置

# Nacos动态配置刷新


# 环境


| springboot         | 3.0.5          |   |
| ------------------ | -------------- | - |
| springCloud        | 2022.0.3       |   |
| springCloudAlibaba | 2022.0.0.0-RC2 |   |


# 探索


共有两种方式来获取nacos的配置

1. **@ConfigurationProperties**
2. **@Value+@RefreshScope**

## 配置类形式


根据gpt的最新理解：

1. 创建出一个临时ioc容器来去获取最新的配置
2. 然后根据容器来替换和新增原来ioc容器中enviroment的信息
3. 然后销毁原来的配置类，重新进行加载
    1. `appContext.getAutowireCapableBeanFactory().destroyBean(bean);`
    2. `appContext.getAutowireCapableBeanFactory().initializeBean(bean, name);`

原理：通过监听nacos端的配置动态变化


**使用springboot的事件监听机制**


监听器如下


```java
@Override
public void onApplicationEvent(ApplicationEvent event) {
    if (event instanceof ApplicationReadyEvent) {
        handle((ApplicationReadyEvent) event);
    } else if (event instanceof RefreshEvent) {
        // 刷新事件
        handle((RefreshEvent) event);
    }
}

public void handle(RefreshEvent event) {
    if (this.ready.get()) { // don't handle events before app is ready
        log.debug("Event received " + event.getEventDesc());
        // 进行刷新
        Set<String> keys = this.refresh.refresh();
        log.info("Refresh keys changed: " + keys);
    }
}
```


跳转到`ContextRefresher`类中


```java
public synchronized Set<String> refreshEnvironment() {
    // 获取之前的配置
    Map<String, Object> before = extract(this.context.getEnvironment().getPropertySources());
    // 加载配置文件到ioc容器中？
    updateEnvironment();
    Set<String> keys = changes(before, extract(this.context.getEnvironment().getPropertySources())).keySet();
    this.context.publishEvent(new EnvironmentChangeEvent(this.context, keys));
    return keys;
}
```


下面是更新配置的代码


```java
@Override
protected void updateEnvironment() {
    addConfigFilesToEnvironment();
}

/* For testing. */
ConfigurableApplicationContext addConfigFilesToEnvironment() {
    ConfigurableApplicationContext capture = null;
    try {
        // 当前的
        StandardEnvironment environment = copyEnvironment(getContext().getEnvironment());
        Map<String, Object> map = new HashMap<>();
        map.put("spring.jmx.enabled", false);
        map.put("spring.main.sources", "");
        // gh-678 without this apps with this property set to REACTIVE or SERVLET fail
        map.put("spring.main.web-application-type", "NONE");
        map.put(BOOTSTRAP_ENABLED_PROPERTY, Boolean.TRUE.toString());
        environment.getPropertySources().addFirst(new MapPropertySource(REFRESH_ARGS_PROPERTY_SOURCE, map));
        // 又创造了一个ioc容器???
        SpringApplicationBuilder builder = new SpringApplicationBuilder(Empty.class)
                .bannerMode(Banner.Mode.OFF)
                .web(WebApplicationType.NONE)
                .environment(environment);
        // Just the listeners that affect the environment (e.g. excluding logging
        // listener because it has side effects)
        builder.application().setListeners(
                Arrays.asList(new BootstrapApplicationListener(), new BootstrapConfigFileApplicationListener()));
        capture = builder.run();
        if (environment.getPropertySources().contains(REFRESH_ARGS_PROPERTY_SOURCE)) {
            environment.getPropertySources().remove(REFRESH_ARGS_PROPERTY_SOURCE);
        }
        MutablePropertySources target = getContext().getEnvironment().getPropertySources();
        String targetName = null;
        for (PropertySource<?> source : environment.getPropertySources()) {
            String name = source.getName();
            if (target.contains(name)) {
                targetName = name;
            }
            if (!this.standardSources.contains(name)) {
                if (target.contains(name)) {
                    // 看不懂 应该是替换文件
                    target.replace(name, source);
                } else {
                    if (targetName != null) {
                        target.addAfter(targetName, source);
                        // update targetName to preserve ordering
                        targetName = name;
                    } else {
                        // targetName was null so we are at the start of the list
                        target.addFirst(source);
                        targetName = name;
                    }
                }
            }
        }
    } finally {
        ConfigurableApplicationContext closeable = capture;
        while (closeable != null) {
            try {
                closeable.close();
            } catch (Exception e) {
                // Ignore;
            }
            if (closeable.getParent() instanceof ConfigurableApplicationContext) {
                closeable = (ConfigurableApplicationContext) closeable.getParent();
            } else {
                break;
            }
        }
    }
    return capture;
}
```


更新配置


```java
private boolean rebind(String name, ApplicationContext appContext) {
    try {
        Object bean = appContext.getBean(name);
        if (AopUtils.isAopProxy(bean)) {
            bean = ProxyUtils.getTargetObject(bean);
        }
        if (bean != null) {
            // TODO: determine a more general approach to fix this.
            // see
            // https://github.com/spring-cloud/spring-cloud-commons/issues/571
            if (getNeverRefreshable().contains(bean.getClass().getName())) {
                return false; // ignore
            }
            // 下面是核心代码 直接把Bean给删了
            appContext.getAutowireCapableBeanFactory().destroyBean(bean);
            appContext.getAutowireCapableBeanFactory().initializeBean(bean, name);
            return true;
        }
    } catch (RuntimeException e) {
        this.errors.put(name, e);
        throw e;
    } catch (Exception e) {
        this.errors.put(name, e);
        throw new IllegalStateException("Cannot rebind to " + name, e);
    }
    return false;
}
```


## Value形式


原理：改变Bean实例化的方式，通过spring广播机制把Bean删除之后再重新实例化

> Spring框架真是神奇，Bean的操作基本都可以进行定制

```java
@RefreshScope
public class TestController {

    @Value("${fang.test}")
    private String configValue;
```

1. 在Spring Ioc容器中，不走寻常的创建Bean

```java
else {
    String scopeName = mbd.getScope();
    if (!StringUtils.hasLength(scopeName)) {
        throw new IllegalStateException("No scope name defined for bean '" + beanName + "'");
    }
    Scope scope = this.scopes.get(scopeName);
    if (scope == null) {
        throw new IllegalStateException("No Scope registered for scope name '" + scopeName + "'");
    }
    try {
        Object scopedInstance = scope.get(beanName, () -> {
            beforePrototypeCreation(beanName);
            try {
                return createBean(beanName, mbd, args);
            } finally {
                afterPrototypeCreation(beanName);
            }
        });
        beanInstance = getObjectForBeanInstance(scopedInstance, name, beanName, mbd);
    } catch (IllegalStateException ex) {
        throw new ScopeNotActiveException(beanName, scopeName, ex);
    }
}
```


在`ContextRefresher`直接对Bean的缓存管理器进行清除


![image.png](/images/fc08dcd81f4c83fe01f57eceb983bedf.png)


# 总结


从这一个小功能能看出cloud基本都是基于boot


而配置动态更新这一功能，更是将Ioc容器玩出花来了


属实是springboot中Bean的最佳应用了

