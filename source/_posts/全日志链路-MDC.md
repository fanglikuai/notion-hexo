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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XZYYGQF7%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T060139Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGoaCXVzLXdlc3QtMiJIMEYCIQDeeZMPUvh7EDWZlctb9rVrhzQo2yVVa2r5o1a6Zrr97wIhAKNYLnOcjeCGs%2F9HpPZm7QBGYC3N9p%2B6KHt6ekTkFXTgKv8DCCMQABoMNjM3NDIzMTgzODA1IgyRzb5L%2BIbC8%2BTtuPwq3APmIYXXo44R%2F%2BPBe4YDzUJFJRxMpC1K76ugLDY4B83Y0btvN2IyJLDbmBNN3hbVvmHWbYzifPGcLRW5Khe1BNtkguOIQO2feN4bbvnLjeXbLZVStdMBngAo5XshjuPiHgWKCZIJky7IUCYLCKvcNjYmxplAjCYmQ8N%2BNMMt0rg0hCeLLfKArtzSWboTRk86UaoPSFV%2BP8WTGivlE3qe6evGJbGINJkBsc8P3LICv6ZX6STE5FmWrMOBZW3cM8hTMVo2OqPL7SHP30GlZbgT6wS27pK2bP5M%2BGMg%2FvFkaPfU4Qz8s%2BTkqdqsBcZX9qWojb4tCY8Z4neFnOCxDhw4upovJpRoUtVvD6e%2FQh9BQWG5tZanwSaQOY5N05RfCY5BemWTQ2YOkhmYBBc0n8TFnKtE0FODGZHGQRwy3WH7u8selSD6AWKhQqvFnIP4NlFaqfmm1UBRF4usHNRDyfiwCEHKyMdLgdW5afs3H8tfzVemLgcK6ylO6cUBMhiA8XZN2P3nEXXM8ZyJJ%2BDt2lGF9WZHi8WwsWiSmiR7IeT4S4eZsxeofL5aQN1eIO3o2V%2Byzks%2Fe2IKsGbAopsXnL6LwrXR%2BKiX4mYxNqldmw8G3kM4T%2FR8kYIPPzOpFdWHqzDQ5%2BDHBjqkAX%2FLCb%2BSp%2BAb%2Bpj6z5osPMuy4oyuoDW9jhENbnCsPeIasw8Od2h17l7v5H9%2FeCCWwdCdE02FRlMQaZ0%2BDm0Gh078RKgB4xnPdi3M%2BDJxWGnzleluX04csYYkfyo%2BYlARHACsjreShKBrybJbG6zGVRBtQJGNa4rDA0oqvsBelzllxAadYpEwUWBkIfwfOybuRd0dT1OYPfLqX7BVG%2Fu8eRPymzc%2F&X-Amz-Signature=9da85084b25f8c7daebfb7a8a20f5e68b1917fdce5ab8c59d4edbf0ae7911d11&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

