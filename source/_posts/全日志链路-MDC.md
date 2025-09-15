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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YU7IWCAG%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T080723Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDZh9HLEX2ksSHnFshVtidMn4phb%2FVcVDIDDiWA3WAwcQIhAJYkYmx8KAz%2BBJA0IR53u5NC8XQKdrDaoqL%2BKl%2FhFMzEKv8DCG8QABoMNjM3NDIzMTgzODA1IgwDTwb8PILZ6oPK5foq3AMIBRoqMqWfbq4mfeGeB2iNsZAQ5jTuZAkAD5GZZ%2FJN%2FTBFWgIhgrbbRXIOIb%2BQRzQsabc81wzVTFx%2BOqeGOTwyDcGmQEG9b%2F6FpB3al6K7Yj13fZbRMa311YeAYmcLvWUx3zr4D1HTDCoy%2FUslzrmu5YN7WKZaDySu6q%2BZsyYwneM4SAUk31dKx7TU%2Bx9B3%2FaOQGFq1x73Nat49HQahHdlXlB8iBp0Wzecv%2BXlCCEn2Ra1bFepXTPozBC1S9X0038IG8ZZlzzjfIblvKbebdxlVHVpI3ALDcDo6LaGwe%2Bd4pcRlsbIXOvhBvJl%2B9xQK04ioHeDQx3QUegCgjETBT03m%2Bvs%2FPdMXEPLKfvoXEp4Ez2ZY27bZLY5JCCh%2BP1BYy1YZQJ0CS8XDAXFwTlXDRdhrYx8pUxK8eAQwhUf%2FUj8%2BUNtAS%2By6LHZJX%2F3EFZ1tmMcTXt5T25kmm0v96pVQ5mAew9AwPhUQdOGjQrcOczfH6X%2BTnYn5VE7SH3cQQnTLwTeZvHvOdmJLs8hFybfMBEQpBxB6%2B2x8uToRwELgZ2wvc2vKVn1Q%2F7ymRJOhE2C85aYqz%2B48BiW4dau0ORPMgX%2Fyy5aI0SZ%2FESIS5V6FRVGxPdSJsftkIsSuGFtYTDc357GBjqkAZ3yvqVQyIeguvtSKlWEPiop%2F0p1205qMD4frxbdpiDZNyu2X2Pbar8n1BU5rj7Ag4q8ExrGfiW0IlZVfgqtEDfiCv5osqfYHHzJ2Xrc8XuM1sRKDgrFf1sYbFh6MReTvM3OHt5w9zS6jvWvBxmhXPGCKWiq1QckpLtwN8pcOnVevGM%2BtvgFXz5Mu31yw3p%2FhvGBE7YMQ2FK0%2B4xvMeX6hOW1veK&X-Amz-Signature=7d4c024b48ab8f8a40368103ba9cc74731ec63d6ed1d66591db725c00de60853&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

