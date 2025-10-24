---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UL5SHCLY%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T130104Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCja4eIq3nnb%2Fe5Yowda0oHWqVU3g8XyLh9vusVjGj5qAIgPBrvM34BpoxYIRw3Qt%2BPqKEfstqkOPV8yJF34LECqtoq%2FwMIXhAAGgw2Mzc0MjMxODM4MDUiDNbmttUBS7OkLyzYCSrcA7xtwzxbJuiGwj8OkEzYGZ8myhUaFYIYw6YR34EzU2uPinIJoetMXH%2BE31B%2Fa8qaUK%2FjyT%2FFUel2YVC8LsyoUpW1tqipLSmLE9MzCXjljYBB8uhLjGZJzaGTUcuWMmckiBEy67zrox8vCvZQ4OkGMgrDSQZ7nGGY5nwp4Q9ZoKAkIUfVkjUU4pkhcKmFEj%2BqmV6RFpYzJqclrewIRn3a5ZnfoCIw%2FMypdRtYj8Nt3cS9%2B%2FlrczkHaQa60yzhew8pCqn9782hqxdQyKmpXFqffPQYnN%2F8jgeOG2d9aUV6%2FwPJXPkm522FgFZJxlo2bZBGrdl12AdTp9wlWBljeZTiKxa3A%2Ft6GZh2M9zOCBLXNy1Pt6jDpLveptpA2tBxeNZyMYMXPOwgCfFmQXJZkbsAkbWtz0406BNHMQYl9b1SY2FsXvNbbstevlkMLhVXawhG2OGo%2FCS2Ofz5AcSt1m%2FVkT31nHm%2FKLLPLUYWcjc3xi7Pkq3%2B%2BCPd0HVn4VE2Sojm4AwBA9EDsdmT%2BokOKOrk0loLWwfQpTV44JpY7jhBuBMCruDwZq8%2BXJt0Nx%2Bby9vrYMga25eebC0m%2FDeKysUV31V7kdYMlaywFnpGCmM48dg%2B3ZWyu8XEjvttt7WtMP7m7ccGOqUBzAFVBId6dQ2sb%2F9zwvNYl0ZYUjAEeuNfQ0Qb%2BRGHPmoCgwEjSxeqV9MN2vr30ZGUzBsdnsr4zmxMkNOZHB2rYh%2B1eEaT9i85l68O9UsaW%2BzBt55in8AdJpTzuR36h4yo2ueWJARZcDisb6FegmFDCR6U4L5iRBrcoSUlEKZNBHSSCx0k7uL5a%2BHsAGjGFi1r3jqY4BdsJ3E04Y%2FcM2mdDpwrLQfV&X-Amz-Signature=050f72a4aecf56e078f6a3a77bd6256cfa3ed77799f97a86ccc4c608a2aacd77&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

