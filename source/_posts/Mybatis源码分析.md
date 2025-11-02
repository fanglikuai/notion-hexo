---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SIIDKU7Q%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIHC0gg%2B3R%2BXLd3VCskeS7DJRZ%2FXNt1z8fnRBVvpm52zuAiEA8sys6zj8B0YSI3qlj6RBVDp%2FfI6qiaqFLPB5FktOYZUq%2FwMIOxAAGgw2Mzc0MjMxODM4MDUiDCzdGcZPqoVaXylWuSrcAy89AQ4%2FJWd8xc6cRp8UyrpsxBVmftKFzXzjNlbbxEhNRcXQxsaKVzByz6ofT8sVhbIUkVbY%2BXtVBbMHJ94KMhnlw4V1WiBrSv%2FUHJgZ4RW0ckE1FCzXCaADndxQItavTNftKWa20M7xFH0qFygSzbfa9HL3q9VIgPDgLvBuw9WhoiflsCVYwXGJFC6PY%2Bb04WK7lwReWDhJex5sTSrZFePm4%2BWJtpLJYtA0AjrHaRVgUoaIZGcLg2GxTVB%2Fy1CwRyGf7cozLf4MsKEuADY4ko57B6G6pIhiT55TZcOzcPgBmuI1n34GvfZPA5CZ76PgyW9ohQGgm4Zn9A8rvgeyY3Ml4qNi1f%2B2Hb7TUllWGKDTX4lX9x4y4CFZyW7VW8H3JZ3CLX%2B2s4%2BEZE5Dm2y7Qv4E5%2BSRTJbNZgtzbAl1G1fGuLcpVlEjDGZvisZO9xXOd53q4lEgX%2Bp2XTxzvk8yy5lPHIQ%2FDMcx17ZZ5jsyVNqIaE05OKXPCRw%2Bhfn%2B52GV7mxQtQ%2FL3BAN19kqgAsPTvtsDgx3v7S0%2Bt%2FRHUIdC6MDmUT0WlA%2BYaT1WjGd5lBDcbw3Ge73FM5fdTBqa91PxxLImrspKT3jB8kpLEqPhNBus1v3jA4gD6jW2NBDMJHxmsgGOqUBwYlIbF8sWF4blHIy05OPvq8PMAHtK3W28%2FaVQRsNmdR4CKJTTRXXQJ9hcdBN482sCDL0OIxTwvUoLzD4BJnO5CsbGIZgGlJyxsU7NWrjVbO200BWfn4FLPGzjmPp%2BogGTcWEkxKZ%2FXVxpCIsV%2FUy24ZqufYwNTVVYqEfx%2FPwmusvSQ2TF7nrzCeYERJofIHFzqylhmXz8Jcth6hgb%2B1XjtVNz%2FE%2F&X-Amz-Signature=f99af6a79f63d7dff45286aa77d185a317bdb76e9a96fdc009fba10415235ce0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

