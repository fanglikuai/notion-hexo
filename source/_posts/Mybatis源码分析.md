---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667AIE2A6A%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBDR%2Bi5MLWzKhPeD4gg37v50W%2B7fKFencrYqX3fgm9QkAiEAgfjGODgHgpQVWeyDpRU4BkA1kFLZ%2BSrK4phblf%2BiwRkq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDA5FuHDj%2BnGDKbSjuSrcA0Rqy0MwJh2zsNYQjjoDbYB1SBBOJ7NxTlY%2FF%2FV%2BQDfRIHR5I%2FjUKxEI8T1jo%2BbtmDBNz8xB2wkS6U8E052Y4QPorgu%2F6ZwZhs%2Bje2dPisuP7TmWcyje%2F2u4u99Cd6DXecaA1MuoUfw55R3aw43yp7sjkffUE4psITetfwoeYguFO0Y68048dvokZ4IJ9KcMbsBh8JTjyPRhhaQMrGJFOl%2BnTTvnnxaJR4Drx%2BCkPe6er5gclM6UrirBavPeY4OB3kS0lEGOOMRyQJZ8hy6MqKbG%2FZpNn1H9hwUCDCVR%2FJjMMsUvsjeCqBD8rvgbxWNGay%2Fu3giJq%2BB0jYJF8AwNUeJTDKYWfX4P3ZRewHsSdbzDEiTDe0Q%2BJYQg7GKa10rDt2JM6N4zorSZvEVaTzmzffeIPvc6NebBrud5RqiocDCOEooZD5hGJl6QFq9sdUCP4e%2FxwjAeO3FcoZiDoGa82KpJzHpRvHnZTsLDUkzB5aQEqI9UDg4dyeSt%2BdFJiBX4PLXnUFVfu%2BkN%2BNbFT%2FxVt1vhPSaJduTQWCg1sIOg0hPEm7Q6Ool28IqDYHlS9Ow7zLhkGA1cEGPhmD8m1Lj2TDntP3L5lFzNHyxyAtwkws8GmwiYOCZAEhi%2BeorFMKON%2FcYGOqUBqQ9Yol7lCF2HHW1p9VGD4SAtbrey6OG5%2ByQntaaLd86fcjpubNmykyXFLsyjlLPI3dZmdTDyT%2FStsuMGVJhzLPHfBFb6uNVNmrjbjiyLGP1QEH%2Fg2zFZnvcPZ%2Fib2ohEqmeflIQ3hhv6dcX8COJfgQ%2FKup19T6igXv8onrVdGBWBPZMsTIcQ0ZwMQpao6mxoqSaJRHQQPMNaMm40JI1%2Fn56A2tcH&X-Amz-Signature=a89c76ec7c2edf9e3419bc082387d0c606b68125e984f7d7395af590c3782554&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

