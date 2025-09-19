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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFYQKDKQ%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T120055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJGMEQCIAR%2Fw0RBLXrwIcH%2F6X%2FAjCDesPa2A6IS5xjCA%2BhalLZfAiBIUlTe91bzwoJuX0TvuGfHi%2FAFMkmTYMLyWf6d9yh97CqIBAjU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMjc6E9I04sIXG4MdTKtwDeqaUOQz6na%2BVK%2FjvNH8DgNEt%2BTiwK9nlGwYqcA5usZh%2BWiuN2CwwBJJP31ba06LvAxEowdooaHxPX8gTo%2F99V63FneNsxQiinaRoopI650kV%2FGL2gaEEWvJfU7DOTNZsdYvCLYbi8muKazsjiHjROHQ6IM2Liw%2BzrYkJ%2BvxKMf3Rn9VZX0TgwXXzhGjLfH19zOVLTUqSjjRdQEFBmP1WMe9IgIlVlnaU3tsUckc3gBFfm%2BkvyYdotrJ%2FcQAWiy7F8l4Bxgp9mHaCraodC6Td%2Fzkx8M6IWuZbSHuC8ndygIq7fykr7n2wKwxX5PfwEtAjR3KQA3KRz2N0EGd3Fqqhkz%2BMVIAEKr9kEbJw16j8w%2FQDd7I6IRPRuxNi4A01njDltQ9CmCTR4qRk%2FmOlGHdvphIprzsTw%2BSjrr4Hn3n9wCJqQa6KMI2%2Bvk9hs5ImwsdHmqkU1Dqe5uKZ9M%2FbR3DhlrAzbdvqpRcOZFbrqvpF4xw3gMDZWNv8XCD2rEqbh1axOwAvDPwTnIQAfZtTlYhuK8srM9Mph56m%2FhEu%2BglMkoIgaAbmWvQl5tpxYPuc3iCW3iKRzqmFGgiWsUrW6K1%2F%2BRGYTwam%2BychKxHBsYIzaaoYUzo6EuIMvQ2uJf4w4em0xgY6pgFuDKMQmYrx22buqaULjdYFSl3spUoHaX73JnM8PLLM92oqurmaZf2HApYs55yKt6MCOBmKWYB%2FhghC38d384SPUZAdGhQVy1OeMtv2TtNONxF3qG%2BDxSjxSbCPRZgRk1htfMOSivWNUgJdjmR7ZIBqhpHp28IBblqR6O%2F9WZk94J8%2FsAClgDxw8TSLnWUHYprdfmPF1EX3aspRcqhOUNOKlP3faSW5&X-Amz-Signature=9706d03ef357431689b63ec63c33442f3689674ca2f5687bc109376a6ceb58bf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

