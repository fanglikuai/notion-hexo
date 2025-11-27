---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMRRZYO5%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T040042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBm72T31ZAFDXOkOni%2FcAe6jubnXWnhdEbRkE1XtrFslAiEA1kYXdJ8Mvdw64V135W3g3aJC2LhdEK2y1aDTbMBqhG0qiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDF%2FMaaYSTRsPGVOgircA6cj%2FLD0O2GifmxORFahwFEZBjTbnloT5YwVJY%2FjS%2FZ7O%2BtaGWXx702U9sYKDvGm%2F%2BMKm9ucMIRI4zY8%2FFgGeBq4N6P2PjP1u1BPuNhMk%2Fztz2F8fdWfgAyCxFZEK3pZanQ%2Fr79HuPxktYMoZ1fci2N%2BHccOrpEGGuFP6QQAAvA7MsHW%2BaeCBIfwWXnN0zGqiMWY1hzD3%2BwmhLwQNZ2p9i8dCPKE9Wf6JJLjcFBfQnxI6EN7%2FQjFK%2FWSNzOXYSqNHUMTqKzZ7eAZNBdQmH7d%2FRP2%2BWXywrlLhU4jRTeo%2FVuVdZB430GV7nS8Rwg7vr6tcmVHEUhIrpA53aW6J%2BBMo1CmMh9I8T%2BCN5AygTQiiJwwpDf5YgdrOafoyTI0LrRzEZG7dlgPr7mjaB2IWa77%2FFlgcKJSCciR2wUuuZyBM5WrL0pTPmoxOlBOCK%2F%2FqPPwwb%2BNeSzyWM1GgIOVepGi0gCL1RyByQP2e8nH3kGl7zTbxm28i0q7lUQoe5ZaCumigq5Jtly3ixAeoB2mNiKK24d2who2PGSU3dl0lJ%2BaWQtKHBO%2FPuEoYf9j%2B05FDMucNabm7TD5ww8QooG2O3wUrsJEv9SQQPZxsGXZ5S5r%2FHcOu3nmjA4IaSsezsc0MJu4nskGOqUBjniCZiku%2FJRJdzU6Jrxx5XGJCAPfJMhB61nT0PWBJN2qnqZ2pAG2Mszsh9eRgD1hxLBnAywDMfdPDkBwlho8aTomWsmWyZQZ%2BMLv8%2FSEcoPxBc1wzzUhGp3evN32xxtb7Y1kbwS8PEOG7DzWC8NS9%2BKf%2FxJbmq96C%2B0yZnosBsIwYZwwGtUKYA8XQVX64YqAt6jvQ5CdHKFaasBWoRdYd9v2sYy9&X-Amz-Signature=8cd22c8967016bb6872dfb07e1eab2b589785149f3436fc9d7ab576a9af29817&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

