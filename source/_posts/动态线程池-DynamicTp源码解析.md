---
categories: 博客阅读
tags:
  - 线程池
sticky: 99
description: ''
permalink: ''
title: 动态线程池-DynamicTp源码解析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/9a3ef0c8-407d-48e8-b7d3-aa3aa449d65a/d3718618ff27a186a0ba957f96444b77965148a9.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W75P42TO%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGmMlaf9u3hMIsQVZrLc%2B%2BcmPypr9Wl70O2eSeCDcqHqAiEA%2FFJcTMSgR9XRNtcFHZBgWCIR1t2Nw08DY5dJEXlDduEqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHGbYm4eMf5JhDrsgircAwmgdsKUfz2sByb3KMyfPO6GCHH6alXc%2Bk0b8FktR%2FMCJbHOwMDjg8uad%2FmA3qyoXknJj29Wo2HmJFkokpowwK5pMBdOyBQAlHvpZv6KyYB9PHwDc0H92m3v8aneysVxqAcIkcr6YSG%2FM0VWBiJ5jMfl2uVFCVgM1y%2BZQol4CjxTJ0KOlCx8XB7fn0uBskPG2vts8alxHcn9pwcflj%2FH0h2R6Qkwb3IQ%2BIILo%2BeaUNiwxM787oZZo5RQEt7fkp83NUo7SIrGcWn5N9JGr3%2BWK9EK6Ln%2BGPKtxptgb57vlmBriUfa6D6s96DRhCcNFeHYN3mGFD7qSL9L2WLr6hciJDgsydcT8r7PDtxyeEN4q5Yoj06Hj2ug3CEADx176KdaOl5tv5y1jWfNG3McbieSNgi8L4YglANhPItC3MN9t4r3ITY23iYXMVUOcctzJelxLImHT%2Bc32%2F5AkL4x6WiwfZlVriLiUEm6q5tj5EWA7RutOjZ0YQ4gwkA4fl98hy60E05fZBXd17vWRCSEB%2FbXBCaVBW886gOWUmKuONOcYKr5BrHvJpZ09nmi7tnx%2FQGwSb%2FYYQ%2Br4oXa9j8B0rgF%2FAZxd7KafLA0mqbjic17dT2BOtZGNkeD8J4i%2FYzWMI%2FqnMkGOqUBc6B4viJEF1yLURtxJ9ASiASwZHCAEYTCVF%2BJkisrtXzLsH5t1Vsmq46mqKhTXzyJpxrletV2R5aWEp3Q%2BQQ1GUWlFFnjHvRV7SmcBO62LLvXKN3KVI0mRlYsiSxuO7BuOrUlzvkZ3aupF8Rlz1ur7rk7tkPnTedLc9BRN5h3X2BlFv7CxbR1jqQWrlrAT3QtWSnswcDDTDAUvTj3IO5k6n9yDqhC&X-Amz-Signature=43266dea8bceb6cfc1548c32cdf1610f969dd1b771820bfa6394d904cc706f7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

