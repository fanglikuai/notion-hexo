---
categories: 业务开发
tags: []
sticky: ''
description: ''
permalink: ''
title: 全日志链路-MDC
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/f8ea9467-1a10-4a85-88e8-30be9914ef0f/129563571_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XIOXRCQZ%2F20250914%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250914T132611Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHi3lkeA1OZq5h4c7FJF6FEMy7yb878OuN%2BlvUOnnXQ2AiEA0TvrnB8pKNUptx%2Fl3mfkj1HaWqTLj8fzJQ15ybZ4Iosq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDOuxY8Y2MxhTm0%2BEYyrcA%2BPhqJ8KrEpYxi%2BWUoUBSj8hx%2FzmaqGWxyNDb4iIyf1rkNdS%2FjrXkJvJQIjxQdNwG5z5BoVQMZHH3xVcKCG9Z%2BDUOTjSv5ONmHAoLZfc2m%2FG1vWUFHAZRFSZtzzDX3dRSBcWPuKgHafapHeACr1%2BTnnKIxpiJ2rRdP8ugEagNfJ4oRps3p6X2RUXXtFfEf%2Fq93KsA4KtqIRju%2Bap1CRIrFb9WCoMUjdqChNI5u4UXVPLSz9mpUOp5GFN%2BrI4lkcg18G%2FEhNody2zagjm4j1z8VamYSmlPiUq8VTdAzxngdH8Aoxt6OdgQWlONcFoFDSZ2BTcX%2BGm4kx1WIsnonGwSgfDBgLxqryqrcq8cGFE4CLMX7HZzbL%2BnpCgAILv588i8WK5AuWwqAVfY5XxocRUqoy3NY3PRMEwxgDlo%2B6Rx0QMe5HpOREpTbBTfdNmSX10wuNJBDPGEdbkl%2ByDXnYhatrZxwuwsLS7VP2LtPFm8sS3zP11Ke8A4AjmqYWHXJMxWUZvJSX2sJo6a%2FcZw9bhIfN%2BE0epL76BUdTvATaaSY%2Fw71Hpxn9rN%2FjapRKX0nW3qZwdwM8XNYjzkZ%2BW37q2C2D75FAQPWe3lwLgZdESGmEagCDKjV6qr%2BsFNH1YMJTsmsYGOqUB570zK%2FXIA1Evma3kmBYSzBAiR1hRX6jtbk8szCxI0WE80fgd1DhJE3yCDvmuzslkg3jCR2o2%2FEJ9nuFd%2BGqCBmBFKN2eHRDJpwpso2DE9Kb92FDRVmUUjCkh5fvNpAaPKj%2BufgN1jfXxNzN2bBWD3sMg7wMe5l9655uczzjN0Eva%2BEV1%2Bq41ZmpDpgA%2Bqg3nSq37mpQX0X5qo0Nct4FWYQgp0V49&X-Amz-Signature=128e94316c4a5cf01a1d9bed8c0d5e6f344047b533e0443322764591c3bc3674&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-14 21:23:00'
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

