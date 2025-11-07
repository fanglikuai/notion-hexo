---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WDHKICWB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICZ3yudo29IHhijihO4z0wsD0RUPAMjDBSaMGP5%2B0SdtAiAt30YyjgZ%2BY5siewpEdeiKzf2y4XI8MN%2F%2B5lTvYMDGjSqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMUw1%2FnclBC68%2BCwenKtwDVnFIR694SU39NCDdd9yRDPwuS709nC9RcThO2%2F4ncr5vUGEiVk%2B9DZQ4FzsOUZUzyCgTqW8iAxjm9ngcQXz%2BA1jpcOe3qh9x%2BcOTONeLBAuu1ngExl0%2BCfw1EnJ4IJ8pUhiUfNOMVgGsgelfIg4acpjoQgugKs7tvFkUF83UGsrdSRDGxUDtNjFfW%2BZiKcZ60RzfJc%2BanXlbYDVIe64FScP6ddxySwwBCmtozSRwQnzZ56GH0R5ivsRZVvPYaC%2FZI0PCjZHcXrWB%2Bp4zANkPzh%2Ff7F6x8p7FJbhyfIyT5vwlEr%2BjF9jLKtnn42uz87DW5rVfQvdbD%2FKuDO5%2FuRa7coH7mB2pToDsSB9WJuo2YjV%2FnWGVjAQE24DC9OTr8mxO7vXwD1Xia68EV9YXkJiIe4hx%2FNcXKZByxCOCPfaIQHJDzniTybBhZkvI42%2B6yWCk58yTdPdGBqAvWYmFCQCoUZxMlgYtTk1V15ZmV7xIS9UOeK%2FS2jglttQ6OCblxlT3G8T%2Ft%2F8y7%2BoPJ4R0WjvhAfrwjSDC%2B4RAReW13ZJGiOuAO%2Fc1rdvtumrTsVfgvBmKYdznRZ0poQ%2FTswRWrRC9E4d5KrWxcQ4EoMFVGJKRGjoF0yth9EuJ4gyCUeUwm6K1yAY6pgHnDYljwctp6OHA99Oo56QqCz8khAvnSnZKdmt0MBtefKKnfflq2cfUxcVGI0Rx%2BUf6fsbQ83%2BrjFbXtghceumnv5312DkpyOQLcEMxXgWI43f5mUxRtL45FrkUxHnIxgb5X6Uv4ctQ8mRxnA2W3JRaWTuye2zCuVfzw483ePkceCDdyhsV82RIRqIRSUDa8vCSg9ljUnV4z2xymSEyjScQ7DxpZt0B&X-Amz-Signature=04657df76ecdf855584769de9e2b6517265877e217b7c43c31510ba0beaad5d2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

