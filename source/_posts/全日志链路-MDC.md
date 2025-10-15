---
categories: 业务开发
tags:
  - 链路追踪
  - MDC
sticky: ''
description: ''
permalink: ''
title: 全日志链路-MDC
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VY25SN43%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T070046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBVHtzif%2BhUmdWjqTCav%2B3538vHp%2FY5GXQ2ah3QheubFAiBczjIasbc3%2Bn798%2F8VDOJrwcOohvIJC4kLQCRLBM95Kir%2FAwhrEAAaDDYzNzQyMzE4MzgwNSIM52s2MY4W7QJqgyQxKtwDmDxB2cNd5fTH7jtQpZJTZtHzr2aQgiArdisJ2%2BERJpNhJDJZa5205PCCAawzqUnvBNT4NRj3VDdstuOo199ANtBiuMSOYWde5W%2BWkw5g%2FOv%2BKqGxAF%2B7ku7IAbzzcLYrEut6TUaIaLoyBzIP4D5d8j8laAD2Kgp23qWNjVxNGj2ufmfr5Xy6pasNx6lWgiq2Hnh6tZ%2BJktmYsfQDkvhS7wzYYYu%2B2r7qyDOYjzvSpraM1ESDL8tN%2BG61iA8j8a1gBU3iF1R%2FOnaqyBVprrt9XOJs74JA53HnYCm3WynmAX8YhjQL2vefsNXpG2xlSfVHy7Fp%2BR%2BfTbvfgm4MMQQfjYho4ZMoJK%2FvtT%2Buh8en1rTQxztXL8wh86lBDUN0t7j0ERpIInmANPsUMvf4fRfxH%2BvrIAD3TWCoNMv5AuOfcMkW5aUHZ8d%2FiM8e5v1bvJl63XpjHXe6DIG9gw%2BmKYEiZj5Z8HzYpTaUYiz3DxzklzU%2B0q7HPglnn6KKJ6WlqsY3xSAiTldF9sui2u6LnBv2w2hx42OK6N%2FX0gQd5MxJCkM6%2F7ZfsvQZJvBK6d9ifmnONA621f%2BEOh641uAHx81CLpiijD7V6yzhC%2F8N23YNvxKLTdsgwL8it9EZ91Iw5Im8xwY6pgFtaO2LNl0aBtle7XKBirG%2Fo6N5hM9P40zjiQalVCzncZCalYRLBEGOFKdEBaoGgp3PnqUEnOsxBtBcOYtH1ls2ky7Qnn%2BB%2F94KVxq6HcdS8fpknf%2BXcpf9huJjr3GohUmnZzC73VpUDVdg2Q5IxPXeEOWR%2BycGxsX6993oN02yFBqW0mUfLCotF6S3N5KZyRQ9Eef0fCSJ9fTL888mxWnSbmi4b8Xk&X-Amz-Signature=734bfee14af101b4cccdd856688b5c637a0b3ec70759128e7607e8b4ddd7ef28&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 14:01:00'
index_img: /images/2757bc02c719f19c0ec634c0cb777b23.jpg
banner_img: /images/2757bc02c719f19c0ec634c0cb777b23.jpg
---

# 基本配置


```java
package org.example.studyboot.demos.web;

import javax.servlet.http.HttpServletRequest;
import javax.servlet.http.HttpServletResponse;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import org.springframework.stereotype.Component;
import org.springframework.util.StringUtils;
import org.springframework.web.servlet.HandlerInterceptor;

@Component
public class TraceIdInterceptor implements HandlerInterceptor {
    private static final Logger logger = LoggerFactory.getLogger(TraceIdInterceptor.class);

    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {
        logger.info("TraceIdInterceptor preHandle");
        // 从请求头中获取traceId，如果没有则生成一个
        String headerTraceId = request.getHeader(TraceIdUtil.HEADER_TRACE_ID);
        String traceId = StringUtils.isEmpty(headerTraceId) ? TraceIdUtil.generateTraceId() : headerTraceId;
        TraceIdUtil.setTraceId(traceId);
        // TODO 业务逻辑处理
        return true;
    }

    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {
        logger.info("TraceIdInterceptor afterCompletion");
        // 请求处理完成后，清除MDC中的traceId，以免造成内存泄漏
        TraceIdUtil.clear();
    }
}
```


```java
package org.example.studyboot.demos.web;

import org.slf4j.MDC;
import java.util.UUID;

public class TraceIdUtil {
    public static final String KEY_TRACE_ID = "traceId";
    public static final String HEADER_TRACE_ID = "x-traceId";

    public static String generateTraceId() {
        return UUID.randomUUID().toString().replace("-", "");
    }

    public static String getTraceId() {
        return MDC.get(KEY_TRACE_ID);
    }

    public static void setTraceId(String traceId) {
        MDC.put(KEY_TRACE_ID, traceId);
    }

    public static void clear() {
        MDC.clear();
    }
}
```


# 子线程-spring 版本


```java
package org.example.studyboot.demos.web;

import org.slf4j.MDC;
import org.springframework.core.task.TaskDecorator;
import java.util.Map;

public class MdcTaskDecorator implements TaskDecorator {
    @Override
    public Runnable decorate(Runnable runnable) {
        // 获取主线程的MDC
        Map<String, String> copyOfContextMap = MDC.getCopyOfContextMap();
        return () -> {
            try {
                // 将主线程的MDC设置到子线程中
                if (copyOfContextMap != null) {
                    MDC.setContextMap(copyOfContextMap);
                }
                runnable.run();
            } finally {
                // 清除MDC
                MDC.clear();
            }
        };
    }
}
```


```java
package org.example.studyboot.demos.web;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.scheduling.annotation.EnableAsync;
import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;
import java.util.concurrent.ThreadPoolExecutor;

@Configuration
@EnableAsync
public class ThreadPoolConfig {
    @Bean("taskExecutor")
    public ThreadPoolTaskExecutor taskExecutor() {
        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();
        executor.setCorePoolSize(10);
        executor.setMaxPoolSize(20);
        executor.setQueueCapacity(200);
        executor.setKeepAliveSeconds(60);
        executor.setThreadNamePrefix("taskExecutor-");
        executor.setWaitForTasksToCompleteOnShutdown(true);
        executor.setAwaitTerminationSeconds(60);
        executor.setTaskDecorator(new MdcTaskDecorator());
        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());
        return executor;
    }
}
```


# restTemplate 配置


```java
package com.csrcb.interceptor;

import com.csrcb.constants.MyConstant;
import org.apache.commons.lang3.StringUtils;
import org.slf4j.MDC;
import org.springframework.http.HttpRequest;
import org.springframework.http.client.ClientHttpRequestExecution;
import org.springframework.http.client.ClientHttpRequestInterceptor;
import org.springframework.http.client.ClientHttpResponse;

import java.io.IOException;
import java.util.UUID;

/**
 * @Classname RestTemplateInterator
 * @Description restTemplate请求的拦截器，将traceId及parentSpanId塞入
 * @Date 2021/8/19 9:56
 * @Created by gangye
 */
public class RestTemplateInterator implements ClientHttpRequestInterceptor {
    @Override
    public ClientHttpResponse intercept(HttpRequest httpRequest, byte[] bytes, ClientHttpRequestExecution clientHttpRequestExecution) throws IOException {
        String traceId = MDC.get(MyConstant.TRACE_ID_HTTP_FIELD);
        if (StringUtils.isNotBlank(traceId)) {
            httpRequest.getHeaders().add(MyConstant.TRACE_ID_HTTP_FIELD, traceId);
        }
        String spanIdNew = UUID.randomUUID().toString().replace("-", "").substring(0, 16);
        httpRequest.getHeaders().add(MyConstant.SPAN_ID_HTTP_FIELD, spanIdNew); // 设置X-B3-SpanId供外部使用
        if (StringUtils.isNotBlank(MDC.get(MyConstant.SPAN_ID_MDC_FIELD))) {
            httpRequest.getHeaders().add(MyConstant.PARENT_SPAN_ID_HTTP_FIELD, MDC.get(MyConstant.SPAN_ID_MDC_FIELD));
        }
        return clientHttpRequestExecution.execute(httpRequest, bytes);
    }
}
```

