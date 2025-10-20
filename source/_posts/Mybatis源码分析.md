---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DPAKRQA%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T160039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCcB9cd0EKnjaupmEOIxJA4uveMLjs0uZtHs6p7SYWhTwIhAJiUp5UHhxSn4yyGxRMy9%2FUwS4upnrlnNwBwy5Lv9XiMKogECPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwH%2FRzla1Ixfle0T7Mq3AOjhXRNLq9fLq5M9XjsyV6b8JdYDitDSxva1pZov%2FQgCgZD%2Bz41%2BIaTOxgAtEELNbfJyFaspSMRU0%2Bjp7wm37OPNE1AVYtmwZix927TL%2B2KDgWRDoxrj4OsmDmmGh8TxPuiEk8vxI7rs91ZDa6Jeur8HokL4etN%2F5YjQ8RRqO0xcEqqG5z4K2NZGhFSdx8tGetf6J5cLtMNKRyp4ZJER0LhufKU%2Bqmyj5%2BBbfRh2UpAV9R5Yd800Ud1xXndRtUy4X6dmURjMTXPCmBYFC7aTwHLU4e64cjC9QtQ%2BsezJvZHRDL08IX7lxLfb8togkLr4wEJ1VuB4%2F6rqMTQ6LDfWYkbAO7lch%2BEcpzTyWTbfd%2B97kSXZARKH8QccEnvPo65DolqFfnzsWBiegdveJZ5XIAGGC4Lq4vrhaki3Z3NXQ%2FoXP%2BqTxnQXRRfeFlmg1QEKtxSd5XfBCWn9%2BVIkvSwtlVxFA%2BISDc0EbtDap6xeUcx4xxvckpSyy3rX92eZWYsaI%2BZ4DQoUVMyuHUENrEZ72ZlaeGMM7QlZmZio4MT%2BntW0QlOjHQI%2ForfqEcHqMPgQ%2FnXd7otYn7LoVihLROAHe0AdM9YqjHEIRwsORhAKPUoX1DUOWHi8pJkU4O4cjCxt9nHBjqkAV7U%2BXeb7a1nhL9ZJ%2B6HQlEeJe57m64T95A07UM6Udt56GbYe%2BDwZ9SB%2BzZR6SOAinPH%2BAccr5I2K5pfcXYS0G8I7XX39vO6ySpPyj0R8SFgufXFEUGr0qKfrx8cFH4FnnsvRejIyYq3DIOMRehGAFkppgs9uFRLQv0t79KIHOlmqz69OL3JdLZ22na4tnfGph4x%2Bi4bTicPmmI%2Fozk7kCOLy%2BaY&X-Amz-Signature=8f3f266737d883839b4a37e753b50dc3a491dd7f5f5c0286174a60993d756c2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

