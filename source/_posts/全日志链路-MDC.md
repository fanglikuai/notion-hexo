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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UN2KFNS2%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIB7D1GQP0FNLwy5Soy6Wg0g2l8PGw2DQ3nAgTtsEtLu6AiA1yoRtWNBP7Nr9HTfgVLoV6bprKsk4Z6KjOi9UXH5dlSqIBAia%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM2Pjn3AThvuQVYglzKtwDjVJbaMEThicEwG3H10TXbetT5y1ksjTaTUQQIYobQMWxEM210XBShHxsKCLiDWDHyibI7JVb28Sti4Z6FGHMug2UPFbFXB5z%2Fekp9sXHhgP9WLKAc8dHAhpp9ofaez%2BvG3DH9XAfyYKc0qiSyEry6wal3cqt%2BRUkNusDmvIVOZQKtapyi5TqgjKtIelMFCVWr2wML71R1AJO8Cez7K%2FMZm1GW2s7AVKU5sDDqJK3G5gbPS2ZJD2GfANu5WyULo%2BeKUqejgJZG%2F65VghnCVYsnDNbbKUhF4b9ulp8L3ViFtSsKt7zfh71ykjEmpi%2B3o0bRSSq619veb7Or6UciXOkIwC391Na%2BiUd52kodwU5ixdSC0%2FM8SidgUuK4jJjySbcxq3rKKX3ah83ut%2BXqECQHg1RhYbnWXTXuLbJO8hCVac8nVp5yJZqG86jHGi2QogfjiNaN1tv7BhprcIjUce6UAiGaEQTUUPI%2F6gxvkVEJM%2FVvjuLd5g5ZW5PIbA60AVfvono4IUB24gB42l0lwa7QPjotVovCAZ1XjdAD%2Bhk7sGmLC%2FthwSl5jrHjb0AC1Sr2Q3Fu2ymPJ97E9tX6Ey7T7W2mMxrnUFEYvR7iI8BduoVyhgdDMpQddcPdUIwstavyAY6pgHjhXiJfrBW9jPtem0dpcVJBgJLDiOGW%2FBnJLvyMZMf4rGtsv2y84bIyRIbNTZGpfNcpoik2XbW3TyB8svPJZ3mjrMGFTlYt2KAa7aY%2Bit%2FwPQVReVlLpTr6JelTdXFBCqqyjhuFiEeQCmx4SWVOPZrP8dYDvZPJ53dJbDjKQiLjilMIALzKTn57P1fOw9KFMvfmcgIamT4TY%2B5S9BtjdfiaNEhqvY7&X-Amz-Signature=fcce176e4f247e124d667f9986821bcc619838b911cebece2ecbc2390b20c621&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

