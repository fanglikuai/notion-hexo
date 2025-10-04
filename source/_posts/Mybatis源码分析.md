---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GZYHXJF%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T060047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIGCrW08A9DgT5fUZjWuXfeO%2BXJZedPDcnxKv%2FHeeR42rAiA%2BNa3hq1KuP1G81soLaCJg1N1bauxEFGkZAoqu6FDBIir%2FAwhWEAAaDDYzNzQyMzE4MzgwNSIMepR%2FUGGEnh%2Bk%2FaK7KtwDIAnEMEPnmvSw1HcXvt38o7igUxKWox4d8%2FE%2Fam7eVD0HfBZVr3LsJuDyFMssHHsxFydiVbAkhZB7xTjfsSq6wytFtmdela3Ks7DCz9SjN3Bup2jX%2FmGCEBgjexZvDl9%2B0vdbrtX5DzPfbQ62CmveE09rsOsgQDNDYvgsij8pkRjxS4D0RECu6O1tU0CSBoiR6m%2BrgvPfgTXEHk%2BFwsAXNfFvsSARNb8OMUr9y6AFCNVh5cDQckFIA%2FeBVYd%2BS77Lf4TNOsZGVhGHNskeY15xUw9b4rl1CRjXSIPIE6sXbfareoPjqSXVA%2Fq0WuEJag%2F%2B%2FxFSFtYvb2fjreUeg4RPtTIW9KnliYTDBVZ4WfN7%2FTUFqoKAXjfFGOJvGeSRyY2sg9TsqhozfzbKBveoT%2F4UwC2tA8jaHcLCoaeBJOTfgyb20RHsX3cOLmkPD5WcGB0U%2FrGDfbsIpOQgR2EcWkOLHQzXgy3O0Ar6VjrjaMI1udMh%2FBLAdNUXwwoOSjzKXWbZH4KhRbAwe2X6pn39RzEHzDjNMyNCG%2FNietDMlnawzsQzP%2F%2B8GjlUu66LAGaxTD%2F6iyBQiMpHI2e8K6jX64YK70%2BEB5BnTBIH29gVNwZgBQUqvqre7WpP8KB%2Fq8EwhN6CxwY6pgE8I8gCjokdpG3jTYbvEO9X8BLrNs6nH9SVKhrvs4NQ8Ne09i%2B10mRwPxbEgfTMPy0OtC7ajZegDJXKQPhLBxz7mpbf19d%2BxkydNBIllAEYvSLHQQEj%2BFOj%2FuwOld8RpIWl0%2FWFfi0Ia3r0ZMsW3bsjnpgbj%2BbCGKTC%2Fro1EzikHDZwsxKfDvzMKXFHdn4Pf0uNNTOqAXKzgwVfENkP7s4BzuD%2BjGp3&X-Amz-Signature=780bb33feb8c1c076b9c9fb26d879c443aea99e9b6e191b2025d1db008230539&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

