---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIZFTQE7%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD4SMhQECTyllu%2BJfJ1lmS6lRka9JWea8kIOnP7QsKskQIgQNzTw8WfHYB6k65GgsS1kvrLKYZNK%2F%2F5ZPv7PVvaeGQqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFcHS9pCeFEo8tMRwircAx%2F46xMZp11WfmblAB7pS1nQM32XQ%2BJeJmWymtZn%2FX10PaHG8pcysr4IcW0Vnlqnngiu%2FIapIZO6b4Vd02PixGzzCl9kdcIIIVIlXtosAy9OMcX8EfivUmQmT%2F%2BuQ8C0sR1ckQMqaP2PR0XVT4ZTTh6JMG5Nze2XC6P3fj9eHKZR86qFuWCNzuu6uB%2Fhl7DN8nPdFGzcDkpiMYVFQ3MqdN1Qs47baysLbKE6vCK3jPCtLaSHsODjLniKZ5fOPhMF3PIzY4UBvFoZvwGCeINhsH%2BfJtcmn2l2kAtkMAsDI%2FvGbGDbcCGq5ru4P5SyS1WrM%2B2vdSIs%2BBqJQALaLCbxlviltpkyNsrSmcVtlwpppoIxvOze%2Fx3HtZmPO0Nb%2B%2FTwPID9uIf9SxLCUWZQvpOos3DD52aORiQgwILQnJtwMUwvsao9peK8Ng94wVoHCDBIl9fnE%2Fz9glPQasI4f%2BEsOsfpTkdSVlGz5nBKNboZd9j8oicvGAoUlRtrYLdSW2RwtoPQZSmmzq0yfis1A4vb%2BdGcuUgJpsHT0VXkVOpsKseoT%2FXlU9CY1rA%2BCY4Q66WYtpfweAknbRBKOPF02%2FB7ChgwXeHa0YlJSQwW2f6RNak5L8dQCTiDmILrfYKFMPDv9ccGOqUBGEJZ4TG0I0llbJmcj3tLyTGnFqPqIsvI3r6yV5Eov5Q%2BX7wlsib4cwt4Bn1HqHpuY20Kru5SydzcXFpsoiOCjD9sXfT7EXnaG6O47AcuHdS9iNA9P2Fuh2vOsS%2BOMT4TaMq6QSNmg2jGln2p%2BKXsmxzoMTK3CpFl4wlGYfq5V7r336BZDP3jB%2F0AQpjWNSsoLX0KDivi9P%2BOpPa50lbKDtRU3ZAw&X-Amz-Signature=5c35aa6dfcb4d6c367da96f0c0767130068d7417e438c55b0d3b6bef1c29bb87&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

