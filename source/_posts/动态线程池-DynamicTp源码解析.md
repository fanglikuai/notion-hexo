---
categories: 博客阅读
tags:
  - 线程池
sticky: 99
description: ''
permalink: ''
title: 动态线程池-DynamicTp源码解析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/9a3ef0c8-407d-48e8-b7d3-aa3aa449d65a/d3718618ff27a186a0ba957f96444b77965148a9.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666Q35CO2U%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T050110Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2FtmNh7ATe7klgqw3VBy8x6oqhmaBhuFnpu8o2cQR%2BogIgPlrNgr41CKGWXn8yKMjFL%2FgIpRH5yqhG7PPNk7N94%2BAq%2FwMIbhAAGgw2Mzc0MjMxODM4MDUiDG%2FUpyCUADmzLiT4%2BCrcAw00xaX02SiqcUtsqnfwybrC5fV8n2UvCx8lzp7hnZfzta60fcX7FAQRoEELA0OU0rsG7%2BCd0TUCsLXf9Uaso6DuQdFl3mPVKnOnjvXiCuBP5wUSM5wRCbAdH1QLF89sIEYqcCVl73pVDpPXwEOddmK1zCfJKUmSn7X9Fa4IbIqz%2BpZsD2%2B18cTcTn%2FoWtrwf0pVUrgxcLRc1bB45egoRVGsh143zg5wSqLui3VGdpqA44SqobXKTsfibU6TiAwy3q07WpjNeVi8mVBvl8wTeahWXt12DLH6gU3sCk8p3k32Nosm%2FWdZ%2F5Vx%2BJwkiCenBwLXUP1488t5SMUcS8e455jBKzdHwa1YY5ymAl9F2LyCn2iuU7BPzvSw%2FXjgbDHUdBnks874ah0pdbymbts%2FoUEkS%2BcdH5f37AfUMv2H3q46YWkiWLR7CdoOic3cXu2knRzq8WJEoE8TDuFi6Ygr5s6gV%2BbfQnNiazcCRtpH7yTeFDUbFwi1vJwQsx1XA2yY3j%2FbpuoWXhWWWpOxKsjO7Kfm8KcbwB5Pl7cW%2BxN6Lgn3ErSYCoMhyE3WKJi0XiX%2FqMiDDumTdewUyg3dcM92S3ms9UFmXHuKM12VBIiv6DGgUoNkka5jH2RKRvc1MLCs8ccGOqUB8g5ceQ0RdRmvynwddbhUWBabx%2B5N%2Bfb0xjUDIFFsH1bRJl4vfQpWhwtJTIs01rmQ9PcglAbvT6OqALahZHOv7ZrTuDvdPiPRaw%2FPlhwu6s13SS6aMNk%2BU9yFb8YR3RgpxnylvGwZW3lJHMgcEZo9Z93iDMjZEVuzbzjxcule5WEyC02iaj6X%2BL%2FSRQEQF%2FUVEupvVud3O7KmvmqcbCSsHVrSZK2E&X-Amz-Signature=505d6f8ed537f8b65ca80f74847ebb27f503dc69948fd53f5e026cfb16cea036&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:55:00'
index_img: /images/85d0f81b2436ec7e37ce37d1acc51a2b.jpg
banner_img: /images/85d0f81b2436ec7e37ce37d1acc51a2b.jpg
---

# 技术架构


![imagesfe024505e9a49bdf607ff5b1d2ccde46.svg](/images/90f93117157cc53567aab3b6e3739806.svg)


# 启动流程


核心组件

<details>
<summary>spring中的配置</summary>

```java
package org.dromara.dynamictp.spring;

import org.dromara.dynamictp.common.properties.DtpProperties;
import org.dromara.dynamictp.core.DtpRegistry;
import org.dromara.dynamictp.core.monitor.DtpMonitor;
import org.dromara.dynamictp.core.lifecycle.DtpLifecycle;
import org.dromara.dynamictp.core.lifecycle.LifeCycleManagement;
import org.dromara.dynamictp.core.support.DtpBannerPrinter;
import org.dromara.dynamictp.spring.lifecycle.DtpLifecycleSpringAdapter;
import org.dromara.dynamictp.spring.listener.DtpApplicationListener;
import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Role;

/**
 * DtpBaseBeanConfiguration related
 *
 * @author yanhom
 * @since 1.0.0
 **/
@Configuration
@Role(BeanDefinition.ROLE_INFRASTRUCTURE)
public class DtpBaseBeanConfiguration {

    @Bean
    public DtpProperties dtpProperties() {
        //        使用了单例，因为这些配置在spring的生命周期中进行了解析录入
        return DtpProperties.getInstance();
    }

    @Bean
    public DtpLifecycle dtpLifecycle() {
        return new DtpLifecycle();
    }

    @Bean
    public DtpRegistry dtpRegistry(DtpProperties dtpProperties) {
        return new DtpRegistry(dtpProperties);
    }

    @Bean
    public DtpMonitor dtpMonitor(DtpProperties dtpProperties) {
        return new DtpMonitor(dtpProperties);
    }

    @Bean
    public DtpBannerPrinter dtpBannerPrinter() {
        return DtpBannerPrinter.getInstance();
    }

    @Bean
    public DtpLifecycleSpringAdapter dtpLifecycleSpringAdapter(LifeCycleManagement lifeCycleManagement) {
        return new DtpLifecycleSpringAdapter(lifeCycleManagement);
    }

    @Bean
    public DtpApplicationListener dtpApplicationListener() {
        return new DtpApplicationListener();
    }
}
```


</details>

1. 在spring包

![images5c3ae976c53c741b6fb4e7b49836703d.png](/images/f382bfa14b7b985e8104ee563bb3d2e7.png)


## 配置文件解析


在注册之前，通过`ConfigParser`接口解析出配置


![imagesaa8f8efb403c2ef5e146c5448933566f.png](/images/4b416a0788169bf1460fe96c081a8c66.png)


默认支持json，properties，yaml


### 配置文件


DtpProperties是配置类


```java
private static class Holder {
    private static final DtpProperties INSTANCE = new DtpProperties();
}
```

> 这里后面为了解耦spring，使用单例模式，创建出来后在spring的后置处理器中对配置文件进行了解析注入

## 创建线程池到spring中


```java
@Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        DtpProperties dtpProperties = DtpProperties.getInstance();
        BinderHelper.bindDtpProperties(environment, dtpProperties);
        val executors = dtpProperties.getExecutors();
        if (CollectionUtils.isEmpty(executors)) {
            log.info("DynamicTp registrar, no executors are configured.");
            return;
        }
// 将每个线程池的配置都放进spring注册中心
        executors.forEach(e -> {
            if (!e.isAutoCreate()) {
                return;
            }
            Class<?> executorTypeClass = ExecutorType.getClass(e.getExecutorType());
            Map<String, Object> propertyValues = buildPropertyValues(e);
            Object[] args = buildConstructorArgs(executorTypeClass, e);
            BeanRegistrationUtil.register(registry, e.getThreadPoolName(), executorTypeClass, propertyValues, args);
        });
    }
```


将配置文件中的所有文件都注册进beandefinition


线程池类型：

1. EagerDtpExecutor 使用TaskQueue
2. PriorityDtpExecutor使用PriorityBlockingQueue
3. 普通的使用配置的队列

## DtpRegistry


在`DtpPostProcessor`中，对bean进行后置处理，将线程池放进DtpRegistry


```java
//    后置处理，获取到bean后，对bean进行增强，并注册到DtpRegistry中
    @Override
    public Object postProcessAfterInitialization(@NonNull Object bean, @NonNull String beanName) throws BeansException {
//      对是 ThreadPoolExecutor或者 ThreadPoolTaskExecutor类型的bean进行处理，如果不是直接退出
        if (!(bean instanceof ThreadPoolExecutor) && !(bean instanceof ThreadPoolTaskExecutor)) {
            return bean;
        }
        if (bean instanceof DtpExecutor) {
            return registerAndReturnDtp(bean);
        }
        // register juc ThreadPoolExecutor or ThreadPoolTaskExecutor
        return registerAndReturnCommon(bean, beanName);
    }
```


## 动态刷新


![images22937ee8ff83afd2754cb4b6dc5aa181.png](/images/f4bbf9f9aa4587ed468a8303688e2984.png)


refresfer使用了模板方法。


nacos：


nacos会自己发布刷新事件，所以直接注册进spring就可以了


### 别的组件刷新


在`DtpAdapterListener`中


![images7b2ba1f442cc65e19c29bb85e1a8723b.png](/images/4baff87506bdbb1c95668b77406d97cf.png)


有Tomcat等


# 线程池类型


在新版本的源码中，好像是换成了`ExecutorWrapper`包装线程池的参数，配置等


在wrapper中，有`ExecutorAdapter`


![images75e3c8216fb9a1d5d6a0f3d48bbbae79.png](/images/64e831ca11f0bb6537a5ae91217318d0.png)


抽象了两层差不多


核心`DtpExecutor`


## io密集型`EagerDtpExecutor`


为什么是io：


当核心线程都处于繁忙状态时，创建新的线程，而不是放入阻塞队列


核心是**TaskQueue**


```java
@Override
    public boolean offer(@NonNull Runnable runnable) {
        if (executor == null) {
            throw new RejectedExecutionException("The task queue does not have executor.");
        }
        if (executor.getPoolSize() == executor.getMaximumPoolSize()) {
            return super.offer(runnable);
        }
        // have free worker. put task into queue to let the worker deal with task.
        if (executor.getSubmittedTaskCount() <= executor.getPoolSize()) {
            return super.offer(runnable);
        }
        // return false to let executor create new worker.
        //   还没到最大的线程,返回false让父创建线程
        if (executor.getPoolSize() < executor.getMaximumPoolSize()) {
            return false;
        }
        // currentPoolThreadSize >= max
        return super.offer(runnable);
    }
```


![images24d005db8a168d53b49ef3b1c6924fd4.png](/images/df2f6e72e1b4b8eac63956f1d151bbc1.png)


# 报警通知

> 工厂模式-枚举

```java
private static final ExecutorService ALARM_EXECUTOR = ThreadPoolBuilder.newBuilder()
            .threadFactory("dtp-alarm")
            .corePoolSize(1)
            .maximumPoolSize(1)
            .workQueue(LINKED_BLOCKING_QUEUE.getName(), 2000)
            .rejectedExecutionHandler(RejectedTypeEnum.DISCARD_OLDEST_POLICY.getName())
            .rejectEnhanced(false)
            .taskWrappers(TaskWrappers.getInstance().getByNames(Sets.newHashSet("mdc")))
            .buildDynamic();

    private static final InvokerChain<BaseNotifyCtx> ALARM_INVOKER_CHAIN;

    static {
//        构建责任链
        ALARM_INVOKER_CHAIN = NotifyFilterBuilder.getAlarmInvokerChain();
    }
```


然后调用到下面


```java
//    构造？
    public static InvokerChain<BaseNotifyCtx> getAlarmInvokerChain() {
//        spi机制加载的spring容器（解耦了）
        val filters = ContextManagerHelper.getBeansOfType(NotifyFilter.class);
        Collection<NotifyFilter> alarmFilters = Lists.newArrayList(filters.values());
//        责任链？
        alarmFilters.add(new AlarmBaseFilter());
        alarmFilters = alarmFilters.stream()
                .filter(x -> x.supports(NotifyTypeEnum.ALARM))
                .sorted(Comparator.comparing(Filter::getOrder))
                .collect(Collectors.toList());
        return InvokerChainFactory.buildInvokerChain(new AlarmInvoker(), alarmFilters.toArray(new NotifyFilter[0]));
    }
```


InvokerChain是核心，然后`proceed`方法调用Invoker


前面是AlarmBaseFilter 里面有一些限流操作


最结尾的是`AlarmInvoker`然后中间是一个包装了Filter（AlarmBaseFilter）的Invoker


核心就是`AlarmInvoker` 然后进行遍历通知


# 监控


在`DtpMonitor`中的run方法中进行监控


```java
//    核心方法
    private void run() {
        Set<String> executorNames = DtpRegistry.getAllExecutorNames();
        try {
//            通知？
            checkAlarm(executorNames);
//            核心方法
            collectMetrics(executorNames);
        } catch (Exception e) {
            log.error("DynamicTp monitor, run error", e);
        }
    }
```


核心就是将线程池的线程的数据转为Bean，然后调用被监控端


```java
@Slf4j
public class LogCollector extends AbstractCollector {

    @Override
    public void collect(ThreadPoolStats threadPoolStats) {
        String metrics = JsonUtil.toJson(threadPoolStats);
        if (LogHelper.getMonitorLogger() == null) {
            log.error("Cannot find monitor logger...");
            return;
        }
        LogHelper.getMonitorLogger().info("{}", metrics);
    }

    @Override
    public String type() {
        return CollectorTypeEnum.LOGGING.name().toLowerCase();
    }
}
```


# 第三方增强


## Tomcat


tomcat的自动刷新`TomcatDtpAdapter`


![imagesd504d11bf87769ed8069bb8e8aace9dd.png](/images/7b0c0d7721752ee05cdd2220b6a9d339.png)


使用了`coyote`，有点需要Tomcat功底了


# 待处理


`HashedWheelTimer`


Handler进行单例模式的设计


```java
public final class CollectorHandler {


    private CollectorHandler() {
     
    }

    public static CollectorHandler getInstance() {
        return CollectorHandlerHolder.INSTANCE;
    }

    private static class CollectorHandlerHolder {
        private static final CollectorHandler INSTANCE = new CollectorHandler();
    }
}
```


# 使用到的类库

- Equator
- EventBus
- mdc

private static class Holder {
private static final DtpProperties INSTANCE = new DtpProperties();
}

