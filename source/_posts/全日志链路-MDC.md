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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDBEHZGG%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T110053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQC9iyWSh5C212lYj0PUaJSyjbLdCcRepVyar0Lfj1g2nQIhAJFth0EKRce6fMsZ32omj3ndLATcNTolrm%2FwkuLgCkD5KogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwv4WDwhYkUUWb%2BSCkq3AN%2B%2Fg4feq%2Bp3xp35TeEbEXa0LqY0MGU00WrdEppFSUNAAJuT%2BSCJ6fj2dNG9HGyGTkfX8nHEssAOmfqDvPg5oJxQt0Q4yXNgZ3iYKYwc4rl9zPzjvsHAw%2B5x8j2EnBYtyHQWUhC3C0Q3phz6slxltAodgU7OW%2Bd2F2xFjZT%2BKeNyqkDE2EXE4t1CALje7ySP0oCT6muDb5F%2BJKIn3GzHitbPnwlveVKBB7Ot2O7dJL4vdgp3zZzY%2B%2FCPK9ztZ0axqNIaT5WU%2F%2BxxIg%2FRKmxuH2cMxyd8fWcuzB2vp8d53a7VID%2FBxuI4ZqznlLUd6WtiC%2BVIr%2FLr491e%2F3rSOs0bFEECdrEjn%2F%2FCBMD%2Bku0VmSrkAh7gTNrlmZkvv166CFgAV5Miz5pFuNAW6AxRljUesBhkezdJSt60hoJoxXdbhBI65QgPt74YB0LDInL2oV2gFFBveBkapHqJ639jcXoAqDg12Un%2BXdg0Kvs93hR%2FkZvdY6xiXqh1jlBtEbzxOArFc%2FNMifcmUaWFFKQ6FEHTOvcuvepyyZ5HYSQQPY5rtYWrTUEnEse5v6HJ6YXmgLwKJgdrx0x2wOdBiHypTITjXPoOXWJf2Cwk7IU%2BLGM9D61hAPKgfnp7QfE12CmqjCX0rTGBjqkAYQsYdR0SsH8ksnDJt70mHYCQnT9H8bLTc8gR8pVF9Nrm5uJ2pn5Hx59n5aLnwXWWo%2FgJQe2QWOXegDUt0iCQd8S4kaUbfsntcS2O2hwBfifalIGmfGRHUMG8hcHXGhmPpsqheL%2F5vWnL%2FSowg1C7Pksbvx5xuacUdE7qjv6Ik27P6Tg7%2FCBXaLAKBZG0g9LKLKcAi6ha9CMFPCfw%2BFfiRZvb9Z2&X-Amz-Signature=aefb9eac186f230cbc45219bd4f41076de7c0d1d3ff6a0fef7b417887c87665b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

