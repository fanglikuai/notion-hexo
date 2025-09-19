---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SV2TJGXB%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCePKFNWJcCH7eTXLSYnkvLuLlJomyJtbVUEDCw7NX%2BhwIgdi8PdDsJBRsDVUrH3fBHsarkpZuptPZz3EYZ1F3AKlsqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBcqcEUAze6JLkdo%2ByrcA%2BqU4bJOcSfHfxosOLfevMmfN9Z542CliQoqFDtImCWwC8OTDHiLKnIvMY6yn808xpZJFCs7jD2LuiBAN4FDO1fyrDq6uDcyuZNBhWEgwdM0mqMh8wLFbeWyYxeKHUNhbIPFdz6fj99MoGfIlKBkkXimmzflK6eP1ka0n7iJfNIrSz2qImS0hHx%2B4W8YIK4gSpP124JncMZNmAnwcpLeyFR9FCJbCz11ZO2g4Ui2LLZMBvpsw6RGk5rhe9qG3KvCC6CapxjPQYAURbnLX7F9DsLHTliJHmrnQRSGRJDdnxcEi%2BEwjrIAaa7rAHTOjhbcS5bty0%2FTRtECIRWKF%2BWUw4yxnRJp%2F5MIXUGKAKr8iJZ4CzBQQEN4b%2FPQsIiugKWUUhdasTMjGqI7hkBaeLzWlrKVVrYU%2FMX7eC9xMFd8Dl8PpwGWvuIACdwM%2BL0cEoHPFoeIg0ss6YB5GNDUI%2BVVVxlVQbiGzHht4SGsX4WyyittnhE1%2FbHSW5xK7dGvZ0J8xUGiEHb0uI%2FkOznvmMqPpq%2B8bpQMoktam199IrhjVSNE32gt4yMiXJQ8MgqpBZDUMOCJNEHHb%2BrRibTINrkJyyurAFq%2FJUFj041bgoPqXrA8pzPFWyQ%2B7o2NyxYgMJDjtsYGOqUBiK8jtQOFWhIwm8yPK3KzyfoGI%2FdiAhCRZcoovF6vYvJloApEp5Sw181eDNt0WCDl5vw9a%2FqtUkEGIS9jckDVXWvvY%2BP%2FwHZRonOwV%2BVTnnIHhzWkAc%2F%2FHbfHkTMmAUfP397nqfAUTcaKRB4mffbVijQbpAdaH56eOUXZowUhFaC3lhIVDY2r2jbphSoA6qvdYpZ4Ab5ssU8obTwiAtY6mmFU3GZw&X-Amz-Signature=2bba5be1e3800f3644ca682796eea1ebe0b287461b8e077579fc5d7db180dc14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:54:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {

    public static void main(String[] args) throws IOException {
        // 1.读取配置文件
        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");

        // 2.创建SqlSessionFactory工厂
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory factory = builder.build(in);

        // 3.使用工厂生产SqlSession对象
        SqlSession session = factory.openSession();

        // 4.使用SqlSession创建Dao接口的代理对象
        IUserDao userDao = session.getMapper(IUserDao.class);

        // 5.使用代理对象执行方法
        List<User> users = userDao.findAll();
        for (User user : users) {
            System.out.println(user);
        }

        // 6.释放资源
        session.close();
        in.close();
    }
}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

