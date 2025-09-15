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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VM4P4LWZ%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T090819Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFmj41ExhqSr1BgnyUIWRfqxDMG7fWB63XMpF%2BNLyV5GAiEAqnmc%2BpB5cJnbsMfMZPyNjRCT%2Bi90lTnJEwiGhOq7ij4q%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDKyL1rvodNrB9niL2CrcA2uXjnbrsKwqyPKdW07JLM9Wpui3jZ4zUGQYohnIfpOAZsOyA%2F5yc8WrlFzhw8x99oOBcYLCNkYY0b%2FJ0Ycxvxfo1pJAVK3XBAHNX5A9vkEinJf%2BzqPgQbUL5c2CzvPOwRL6g2dNh68H%2BESlUnbKxriqpNo6MwBEecN0Yi55wOcH9kNKCbpkOWkCvB%2FWm1ln21oOy%2BWSO%2FAqmexo6%2BNB0f8p019e4V9aZOV7U1wBLhY%2BizFSxwOd7h0jdCCgE9VVNjunRb26QAIvc9J%2Fh1pNmE5%2FiCi0q1RVceT6LNXjSc9DP1omf0nvQcgUhtjJGHlHauEkODqPeFFcRURfI1LSn5FRUP1XK1CFZmidyUYo9wT6EA0F7vYZTKLvArIdSqsCvFTD5EPQ5g6WKWfynzA3cQ6jJKswYG2AkMGSEK1vXmzXvlKROV%2BAqJNmTSRJXtGJfhrS5V78ywvJisEJq8f23tPa63%2FuHkvPQwYj14FBwoSVMk5IGNcB4V4C8haWYBSdDm6GgsB15%2F4dd9CPnx64b7IKpJCctDQgYGmyiw0nhjDvsu1JY6z73M3Rm7QtmmT%2Fapar4TX%2BzVASLSOlHfAgG7FliXHc8hZR%2FLLLs6wH%2FkodHhqyN%2FSbqtkqtCVkMJGTn8YGOqUBXUm20HEiXdSKLviL6K2Pv4us3zuPuTS8hsu0lJMTnwre4gH9u0f8eQFwrw3o3DxiioXgG6uuLZSw0eiQNUSXoaX45n9l0kRHHI%2BWFWkicYxuQSNBiPjQAAWu%2B65Ml6z8v3WMKiOjBTLwKvuO7l3upok9oBaXyRAqDu863Dy3nKtKd1VRT7OnkTCjsndd3c%2BhwwbjqQ5peOS2PpPdyjoV0E8LkZEp&X-Amz-Signature=77e5c2a9c712bd2e5942f15d61771b0ff3008ad247e60cd549fe739e4bef0aa2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:55:00'
index_img: /images/2757bc02c719f19c0ec634c0cb777b23.jpg
banner_img: /images/2757bc02c719f19c0ec634c0cb777b23.jpg
---

# 基本配置


```java
package org.example.studyboot.demos.web;import javax.servlet.http.HttpServletRequest;import javax.servlet.http.HttpServletResponse;import org.slf4j.Logger;import org.slf4j.LoggerFactory;import org.springframework.stereotype.Component;import org.springframework.util.StringUtils;import org.springframework.web.servlet.HandlerInterceptor;@Componentpublic class TraceIdInterceptor implements HandlerInterceptor {    private static final Logger logger = LoggerFactory.getLogger(TraceIdInterceptor.class);    @Override
    public boolean preHandle(HttpServletRequest request, HttpServletResponse response, Object handler) throws Exception {        logger.info("TraceIdInterceptor preHandle");        // 从请求头中获取traceId，如果没有则生成一个        String headerTraceId = request.getHeader(TraceIdUtil.HEADER_TRACE_ID);        String traceId = StringUtils.isEmpty(headerTraceId) ? TraceIdUtil.generateTraceId() : headerTraceId;        TraceIdUtil.setTraceId(traceId);        // TODO 业务逻辑处理        return true;    }
    @Override
    public void afterCompletion(HttpServletRequest request, HttpServletResponse response, Object handler, Exception ex) throws Exception {        logger.info("TraceIdInterceptor afterCompletion");        // 请求处理完成后，清除MDC中的traceId，以免造成内存泄漏        TraceIdUtil.clear();    }}
```


```java
package org.example.studyboot.demos.web;import org.slf4j.MDC;import java.util.UUID;public class TraceIdUtil {    public static final String KEY_TRACE_ID = "traceId";    public static final String HEADER_TRACE_ID = "x-traceId";    public static String generateTraceId() {        return UUID.randomUUID().toString().replace("-", "");    }    public static String getTraceId() {        return MDC.get(KEY_TRACE_ID);    }    public static void setTraceId(String traceId) {        MDC.put(KEY_TRACE_ID, traceId);    }    public static void clear() {        MDC.clear();    }}
```


# 子线程-spring 版本


```java
package org.example.studyboot.demos.web;import org.slf4j.MDC;import org.springframework.core.task.TaskDecorator;import java.util.Map;public class MdcTaskDecorator implements TaskDecorator {    @Override    public Runnable decorate(Runnable runnable) {        // 获取主线程的MDC        Map<String, String> copyOfContextMap = MDC.getCopyOfContextMap();        return () -> {            try {                // 将主线程的MDC设置到子线程中                if (copyOfContextMap != null) {                    MDC.setContextMap(copyOfContextMap);                }                runnable.run();            } finally {                // 清除MDC                MDC.clear();            }        };    }}
```


```java
package org.example.studyboot.demos.web;import org.springframework.context.annotation.Bean;import org.springframework.context.annotation.Configuration;import org.springframework.scheduling.annotation.EnableAsync;import org.springframework.scheduling.concurrent.ThreadPoolTaskExecutor;import java.util.concurrent.ThreadPoolExecutor;@Configuration@EnableAsyncpublic class ThreadPoolConfig {    @Bean("taskExecutor")    public ThreadPoolTaskExecutor taskExecutor() {        ThreadPoolTaskExecutor executor = new ThreadPoolTaskExecutor();        executor.setCorePoolSize(10);        executor.setMaxPoolSize(20);        executor.setQueueCapacity(200);        executor.setKeepAliveSeconds(60);        executor.setThreadNamePrefix("taskExecutor-");        executor.setWaitForTasksToCompleteOnShutdown(true);        executor.setAwaitTerminationSeconds(60);        executor.setTaskDecorator(new MdcTaskDecorator());        executor.setRejectedExecutionHandler(new ThreadPoolExecutor.CallerRunsPolicy());        return executor;    }}
```


# restTemplate 配置


```java
import com.csrcb.constants.MyConstant;import org.apache.commons.lang3.StringUtils;import org.slf4j.MDC;import org.springframework.http.HttpRequest;import org.springframework.http.client.ClientHttpRequestExecution;import org.springframework.http.client.ClientHttpRequestInterceptor;import org.springframework.http.client.ClientHttpResponse;import java.io.IOException;import java.util.UUID;/** * @Classname RestTemplateInterator
 * @Description restTemplate请求的拦截器，将traceId及parentSpanId塞入 * @Date 2021/8/19 9:56 * @Created by gangye
 */public class RestTemplateInterator implements ClientHttpRequestInterceptor {    @Override    public ClientHttpResponse intercept(HttpRequest httpRequest, byte[] bytes, ClientHttpRequestExecution clientHttpRequestExecution) throws IOException {        String traceId = MDC.get(MyConstant.TRACE_ID_HTTP_FIELD);        if (StringUtils.isNotBlank(traceId)) {            httpRequest.getHeaders().add(MyConstant.TRACE_ID_HTTP_FIELD, traceId);        }        String spanIdNew = UUID.randomUUID().toString().replace("-","").substring(0,16);        httpRequest.getHeaders().add(MyConstant.SPAN_ID_HTTP_FIELD, spanIdNew);//设置X-B3-SpanId供外部使用        if (StringUtils.isNotBlank(MDC.get(MyConstant.SPAN_ID_MDC_FIELD))){            httpRequest.getHeaders().add(MyConstant.PARENT_SPAN_ID_HTTP_FIELD,MDC.get(MyConstant.SPAN_ID_MDC_FIELD));        }        return clientHttpRequestExecution.execute(httpRequest, bytes);    }}
```

