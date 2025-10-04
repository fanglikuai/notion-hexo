---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PGRT5I7%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T140059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH6y0JYeE2wIxIFUwk1c9ktqse9y%2BE1woxrDSYRbu6JmAiEAhYAWgA7uUMMFJ6viZThowpJF8sr8hE8YOCgOjBFX%2FNIq%2FwMIWxAAGgw2Mzc0MjMxODM4MDUiDEdJKeE31hj0NfOsfSrcAyQVhlkJlmAmOM%2BHgx%2BqAAuUeR4BlQI%2F9KJ6qqnBmQJOYPdCZQx9cN3v5QO7DFj22kMThi4GsL%2BTUgzxFj4XvnLK5oZLgnt22LKhapFvYsNRC%2BwbOIgvWyGM8bW9sTJtYRuNMeeVSTkj9HtZil1JxYbR34KFpSOAbL6gevEgTQp6cp7umH6Kvz2G2rEQYNSnZOpBBNP2nEZBH2S0K%2FWlsKhzxm9l615GvuVgr%2FUJ2VkJ58F9iMEJRfAARwxSJbnfPgUYBEat%2BvknwrvhC3PiSUwAnH77ITr3a3TsH72ZH0uegUpbyiWnV4YojlYbpekAjXx%2BDzE7fe3Wbg9AmYXpuD7KhiZQO2IXu1SnKa7S2xMKcqKT41rnECt2NUhbWaryStG%2BShhs0Xkmxb7jL6bEUoCkVMlJzYT2X9Tb16RqGrpcXd%2B7boqiq0UAm4StffrS5so2J9%2B4QQcJ15yqadahwBvAwoH1HcB2w5pKPHiGFtl3ZtQZjNNyePUhL88bfAs0lmL8xWO1i%2Fsl1lyLL8FtBiouMCx9I2a3Nmar4Q%2BxIKEjar%2BZrP8oYkwe9wWUDICmk2Wpy6ZPhBr%2BqCDrn%2BoCpjgJEpTo6l83%2F7WAd2TnO5JJH5dV0VdLUtRUGB9NMKfhg8cGOqUBweaSpVrynROPZ%2BcXkITv5f10r6YS4I44bigaFtS2hQHPXPxMH5n94xIpWFdnU1eaQHUe7SOLn89J5etyoGHDDTriohRBnQyPcKxSTspr%2Bc4BF8dZfZHHlHl9rZPsSj43u04J%2BeNaKbRkrB8JR1wUhAExgOAqp0TuACsxfDZnJBUyg2JrjBfimU0SyK%2FCAgBGd4OR5aQgCvfvTNALxoF0tVkvJtYm&X-Amz-Signature=c79714d0ccf6abcec89fe93f7f082450914b143a9ce315d8b3d4d87e6c061984&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

