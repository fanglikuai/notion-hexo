---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662R2ABSHI%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T160132Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCcOvJzkkZSsW69HbyAWPiw3QWNvmSD8sTE9e%2F0nTX4JwIgajVS6gVyG3yzm%2F31%2BDT1x6ckKajs7JTzX5OHDbXlfvcq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDPKAIBsaauc18sO6zyrcA4DSWwLMuyDejIV6Ql4I3jrPbYa5cw6VKt4rsb7iVVPRmXx8i1E6Ag4sDvK5a5YyoLLnUHg%2BrVywX30hgiSLTIP6XBBv96Bs4a8sXbLNUbU4Ng8YtmVFX%2FKWuWKtlBFMnzD6XwVx6SfiuAwuY770MVN3pPKILY4XGe3Na8HwywpFO5RvUmGq6UJnCxnSISQSQexQmExfNvVf1T0%2BkrZsMyiGsgD7iv2Ye%2BBbAsOn2HU%2FjvXpEbR2TnFraLmUWr04WCKWc4L5VSbXp8kI5Tn1wbBODQpZ%2FYEWhO9gVlAvwSM14ieiVhPd9kLEEgpMlap%2B2%2BhuHPkylo2zaPSVSXEa%2BBiWTpugtzpGOI4DYmcKJPrzaT0uRT%2BUoxOnqHIxT1bLHVI1Fl4xEvDBvr9qtKwnPL4HPSSSReHwj9KKTzDVwHlKZM8z3TrGxwLYKb%2FiY%2FIZ0Qq4ru3LkF4%2B1FB5EXjb51CUWnFfZHuW5q1lnOp1%2F2uqu5V5lWT%2FnzTh9FXNogoMZJE0OnZZ4pWAonSAu6jGK%2BLOJdgTFt8yLSruB%2FpqF07GR8pyzkJSzeD9diOOLEz6T45TZE82ErsErA25wupa9MAlOC9QNJ0Yb3Fd%2F8uFCYIUH%2BdonnXfNuEiYF27MNnf18gGOqUB9LU6VE80cCUlQySdf9yYrxK0A6Yi26Njh7QInBxyNc8KyPIq0lcg1vq9rLDDOTxyUm0HpfBSG5x3c7kLEY6IKjK9FXrLrqOa4dzgTFpX7HoHh4aAwP7AG4kT8QQNsNaCYODueROt3VYcT%2BEqWucqsk0Ta6Dl7IQhNoYfWDcZEAYDAgmgvQTHWvAJL%2BHAaiHtzGDaFPcHcfuBpSdbmFRxF%2FUWWer0&X-Amz-Signature=8bb5ddb2f7aa715fcc940c4a65542f17b26eb74d16039049891da8e20db01b71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

