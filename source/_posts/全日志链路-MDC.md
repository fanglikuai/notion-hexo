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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WY552B3L%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T153712Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCv0Bahu2zKjPQax%2FxWWrRag1ME8DyZzKmTWtKQRRXRPAIgf37O%2Fhgdl70yHHoVBFZfEItZgcf72QE6Jc72Pa4FEt4q%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDL9VoJ60fMA3H0wZpyrcA%2BQFkO%2B%2F5PEHboGeno%2Fevi3X5Ay4yirBAmsNz97qO8IGbX4tEYG%2B79F6kS%2F9UgCnk5F4F%2BTLcZ1SboH9x0k3qBgjfWljLuRFOqHaEoNu8PsQEtwPsALFn983kCglc3qdsR%2Fq7ZhRY1NYlONJfS3m9sUv9KcSCjpqlSTZIQPES9PI61PSAWKQt7qapcMm%2Bo6mMd04jXxEmBuBqzZwdtNhhiPsAOiBG7qzJHmFY%2BMD0VEsI0o5QQnRa4fajlVybVo0d1gIg0YSpEHVe60wyewnHg0H99dgvFYfJWfl3VjLw0FE1Og7gViP8Jwl2Ih9XmV0Zv1prv8MC8a5xq9LJuHdI0YGhZ1vG2kOWiSiz09bXSYu4Oe2rSSEcobcX1jEe%2FqZ2TzylYt5D3OjQ22vObAnlHqbGKQW9toaEfPS%2FtjC9e9g8WFXKEdA0kBhjP7hFsKjBZFBFEvNZIuAoX0c%2Fdv9fujiu9mUSQRf8i6mUo2PJmrDUBWNR0zhyQ2hIBLoRQBtPHcNXdaXOhVOmfbPCGgSW6bKKje%2Fxlh9nswt8x1TWAErhHXLTAfT%2BrNHhuBsihL8PWQ79Sgk1qnf6z1wTPsDKJgfWX3WDzkcaQ9OLQaesO4CP9FcPJcQu0K%2FelIYMIbboMYGOqUB2SxFBouMrS4VLlsbe7jDWUgreCbOnmcMs22cCD5KCPm%2FFx2ct5tkd4nTcwebUHB8kaoKg%2BsEvHR8l%2BJJdT8zfattjvy%2F8CwUbQpBW9bSXl82%2BZG%2B0Ju8LfnAfbF%2Fi0LiZ%2FyH5xu7Igb6rH9IHT3LABqSRKOJHvvwZrW0iJeA6BlcOR9jzhOcU2v7PhDmrtdR3ZGUiSeT1XgpvQpyA80daP86to9q&X-Amz-Signature=666b7cba4d6ac85da78b10bd4fa99d8714a252f4ac0cb48c5dc8fe5bc790780a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

