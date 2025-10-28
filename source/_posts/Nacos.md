---
categories: 源码阅读
tags:
  - 分布式
sticky: ''
description: ''
permalink: ''
title: Nacos
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/d628cfe5-f52a-4092-bdc9-3b45d5c596ba/121397145_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663AG7JXZQ%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDcvkWUjWd71vMqs79xzvHaHaAlzJSkbftdQmzHFqbqKAIhALBWb%2BZAVFH9YD6%2FsWVAHR8ZXjZK0MBxB5NhSS4fGARuKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyhM4nfrm2EsnVPbVgq3AOSHK2MeEuA1QvNwUXS0m2IXhO%2FWlBR%2BNyunWo%2BAduNxd2shLCYOQwnN6Ize6DfxP3bT4A%2FOQFu4dUVhiDQYw0XvblfA2lKBSX5dBZT0N%2BQFeASkiMJyZt7q%2BjvgMOTHJqUn7qjsQIayHXe2EESM%2B%2F0%2BbFZKxmVU%2FyXr4xk7KuVF%2B1Dqx%2FhZhm33vdxdNSEr783483%2FVgcN3q%2BnaOtvISiKxnZkDpw7UqU0JyREFQGikn5eg2BOLuHM2Ae1WAmXW28GUOI0MBAOy7tGX9kZ0dmJXPb4%2Bgg6rrpLtA9AsNJ9hJUUHZS8uomXv2JyY6HNyq3Wrt32sjZNmf1TRrrLxg84Uj7PvcFJl%2BlYqszrEHB7JfncICF%2FfdKWrKo3ItvPKNPru4mykxU3kbhLdn8ScoX4FOpEKNtUpxW%2BFXbTVd0%2FO39ANtvzWBlAOgPkjzn7VcwEV26B4AlTBukY1l%2Fc33ayBF6wo70kUqpD2ed4pcAUY0iJ6R0%2BBnmxBCBzNEo%2FYLQUYtnUplQSfdWTGzWKRAXTR%2FYdDqjFwmk5Aiy8lbaNAgjTLLdybzff1G78NqPPi0FyOYxgnO5ki7cSGEAN9dtHEc2oaAxwpuXmCA6L93iqNZMpR5Cy80vEKdgomzDasIHIBjqkAX0gAfVsbmvvvyhOK2r%2F69hmyT4xkxGdv71I2TFRHlaFXF6M8m%2Fesd2RbNiN%2F0K21VefsJL%2BdM4y9M1gtrlmr2CJurw1wcbEbnXURzJfOcipQ%2FYg%2F01ryx3VhQNQHTxWDZW6jChlsvHyTnzLRSV%2FreR4GCaUNnvjKOEnm5nEgN6SjrwI%2FYVOtW9KB7u9bsQPZBksUexh77sEO1hV1t499lPmO9Zt&X-Amz-Signature=724c6c5018d424863b021bc62855d0721dab5afe953c4dde37d8f7f9e58c49b1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

