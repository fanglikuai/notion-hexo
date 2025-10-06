---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QRBURS2X%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQChsxTjNpzikR4Iw9gNoRx1UdBdGlmz1Y70HiPT3St%2FJgIgOMpoOYyB5C%2FXMKDVW6Pux%2B6dGlYscIAEJcNg9mkrt60qiAQIgf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJhUmmRATyB5ZkmfYSrcA%2FvzVxZ94NcrmVjqs6htn84t7EPnIeYlPSC9294Uk%2BGFYJgf61UvRqSV7an09IYulH3oCQwpIYCSDuHzKL1xqttspaCeJWatOAI9xtIYgS9B9zV88DxrKrMcHflV2AHkNAeFNthqugGsM%2BDUn5qZZDQcBMFYjjJPI9cqtfN4CKKPy%2B1FZHvIoAbX5Yib%2BN9o5DoENy2oMQDnPwLHj3QLXYN9sXqdKQfkI83D4q1h%2Fnk3GFamu1NXt7ELoZ0UD%2BS3c7MK%2BLM4paYiy8yweKviKk09ayM2kLNQTtqxACJ7Vy34AQ16XcYBQzC86FwmrhYtCJ%2BhVqyn%2BFGYp8ke58JTQ0zW%2FksfOA49vu8JtY09rjarq3ZaUgR7Tb5w2MuQW777oqFeSMUmXTM9GNO8npxOZJ7CMKp2YTXvf2Nvf%2BTk922%2B3zYSHixXgFE5YjoelrUsNzJrh7r86ic7xlJkmO4OxxwsnFBGl%2Fui%2B68GRowzrNEUqx5%2FiJMX%2FxwmNvwD9dbabqR7ruFeVI7pDwvG6XNEniX15XGMSFFl6BYV8WichybzPP2LguTfZvw7lBHTJQtvkIHhBZZFkA2SMXVha5mGaC2kJ35ZWPi%2FcOlcAkXTK7vRrVMQCDW0T6r5Zee4MMX%2Fi8cGOqUBapsrN4N%2FaA0XNKLO7%2BlwSJUPyCbfl8purteK3%2Fx7RGT3AOtgaVNFjOf7qsaUtgwIJ5%2BIe60Q1aQ9tRZn8N1gNFUYaF8syc9iIV2gJoGaBM8SF6106Z0dg1ghFNHSJFDqdtMjTphye4IZo7c5LQSAUD3ej8I0awlaV78LnxhG3IACXp8yxPjOwmV6XRi7pexKZvnRDKC5v3aCajJJLXFEqXzbr2Vk&X-Amz-Signature=b5beb10462c221fb02cf5cfcfabb60f7a27710be2519e3013b9828f36a95dd0d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

