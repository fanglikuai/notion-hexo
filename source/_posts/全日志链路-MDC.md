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
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PCZAVSO%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T081019Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCID4X%2BZTKiResaPMurPKYsx7XqPpWnBqj1%2B4VEej2a%2FdsAiBd1ta3zo%2B1VLdXbIRpumxrhAjKGOopcEI%2Bp3Hh%2F6qDzir%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIM6nGuRe4WO9D59n4WKtwDPSCDdep1nYnjAmGlFw%2Flm9ECrbMUC%2BOZLDhE1%2Bwxk5OW0nt1%2FhU1HhMH%2BooWAZrndgKEs3PoQZruAHZ%2BE9vPTYxbf4L%2B5Glx3cRWANzMeDurEQYQOYhZkjH56hjXDujwWvK%2FvWv5QLWBcHFsIKE5cCA9xkE9jAbUOByILvA3ttoPrgpBWncagi%2BEQlbvqGaLcAAjDn%2Bokq0kq5bjKFbHKFZqEszZlYTnDC6lGV3bl4oPoZsC%2FCkz5gTFHWVTP96qSMeIQHvRZo48y35LcWu8LqhHmWEBrfJezLW%2FWBqsUlrVZTPdA9wOmYBWJTHsWTDABG%2BTPNIvqPycPZeMrtlf7HgS8ELcHvGF9%2BmA%2BunTK2xYIkOduoX6dgNvslx6madqRypyYrK26MOxenbxZwGNPkLWJQlp5Dt0jQhO%2FiQEltuOqglvJgeudxZVD9V7Lq%2BbSuDmCCsqxhMTnFr29gkEXaBPXfsDzZShc9N1%2FiggfPUtun0Py24E890fFTCYitFHff2bv0nhY8dKy15D23yZtjc%2FC%2FbBOB%2B0%2BTgHfsR4wQUut6AcMmTemKEuPnDScEAoIGwsp0fpAk0oxi28XSHZy%2BkzyROmA%2F297%2FhDqD%2BhdLrxfkWP7KYhSnEK8q8wp9%2BexgY6pgGDPTINTNBTpMq7e0VDZ9NxQThBVNK3o1XJmt0HSeMDCWmmV1vGbeJ%2BKSvtKAV62JrgYAqfP6kcCsooOMwvXlD4iSHws77GYquljZB7SfAYDG92qrWlpr8RMQ1qWlhFxH9ikAPVm1NX51rOv4bMU%2BDxfYXwVXlOnvca4%2BcwjEFABUoOz3ZnVp9t44yglT%2Bu6xEBHYamZeLeIHEPsyawJkbK9KOgDUoY&X-Amz-Signature=fa291369806186e5f6fc330adc04cbdfe5e45cecc3862dcbcb4fc9ee8ef197c5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

