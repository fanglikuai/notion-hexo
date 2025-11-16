---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667IO3PSPG%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHAY2MvkgpqjlWw3gZy7%2FUqo%2BwixfCmqoRMFwhq2IS6XAiAnI%2BNG1WjJ%2F1htaQeQxa%2Bo5Ub41ZotCuDCNHqnIFZ7vSqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXy4cHO7WOzUMjWbjKtwDD6ZBToq12etaQvj09tY0QobtzTvXfLRxmNnsdy6X7Ur2m2zaL%2FYdSH3ZGgEQxdLVj1k4UjjbpT42Wj%2B4Csx5QJ8GFDb1z18KDY9rr61fvOupyvZtQAjhCcMe5sSCTxrqouIsqEN7LUVuk2c4gfVtkmQ7Morcmbr1zyyJqDNAXwg8voQWziYCUe9EUt9EHDhGqr8RQGNlJDIzj9ad%2F85IKvyxmx2448qtkDkDjc2TYWT3T7KIZ6a2fUnrrWP0g0i%2FgNaCFpvQPtf9yRvA2IC6Q9WccH4Bf%2BG6NUoIp2vhFWIoBMtM48l%2FfvQwTT01WBVEErOyRogyHkT6L1%2BDNMbP4L9Bl%2FnsaW04v%2Be4tOI0LdWiBcnSFIgcJ2UUKq041mOlsUqQSBXRaw11yI4mC63I%2BkqGmQynk%2B49KwCQHAqqaev5JMRikhGxhsiutNgaLqzyQwYkyK%2BenxSGXgc8YiWSSaBzAXMdIDbS2t91%2BZpSPnSz%2BHJtTz4jIAW7AWuasn387F0HxPM2R3JTr17OQI3EhNeq0yhZOay%2BX2SpQ8rftMRJ1cIQK%2F%2B8FrZqFb5qcfOxp9Ga8WSS%2F0GcZml68RJu4ivDlapKjIN2yiFF49F2gB2J37wp4H4Mi93Ioa0wvd%2FnyAY6pgFcp%2BFeZxKLDEb5dQvqxg3htkwwT9BPR4ye5FM%2FyA2XIf0pkWc9TVpIXXe%2Bs%2Fje5oJvMNXnQz0Y6SJE%2FzB%2BaaHBv7WuvJdqxzwnz3pAPwK%2FG1Uas%2FrJZ9MpG7x1UTBD16VLnm%2FyaLr%2B%2BvBwraVeWkYu30wCsGP37w3Yw7w256pfUtP%2Fre1KF5QnMN9IWd9DgBdsRSE9NNww1cYHeb9jcBB8IaFdOrex&X-Amz-Signature=0fe49cb8ca9f1a3844e922a14eadc2e95beb3e15ef44d2355598d3d4c420244f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

