---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UZDJFYD5%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T040040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFqDXs3Ujis4sXtAD26h9gWdIzsfvU1laU%2FiDDVWTHakAiEA3RyXafnGVnPUurrwzDsoQ7vcHFWKILbvE2Xgu5WyKkoqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKmL9JFGjEOyp7TwjSrcAwJ6whGZ5Nb2Q%2BjxNvF3SH0tXaDPG6oK%2BepvJOY%2FWpL3KSqTBYspiG8bjgFmhEhI74hbT2AdZn3SiFIAPe4bLbuGeWkNLR8Hu1OIhGGrjNjEo5ZM%2FmTqyrvhC9lQvrM9%2BpW8hbEW23LVWF%2Ff1VhSyj5lpSHgZP2Bjm3IZW6rptgxPQHxx1LqntkR56TS7rvlNRjjQlwxuKd8d1IL6LouX5Gz%2BO4dJzNU%2BKc8bO%2FORBB3zOQq59ma9tUJH4ir9xHxUmUxMtZt6Kp64m8p7a5%2FF9U0PAi08KuUOXTEy9blyy6ciSePPIi14nabB%2F6HeEyWYgcDn9%2FLllRJopxNEvBGRJDHB9uZoyKQK7LPHTGl7SEvtX%2BcgGOQsn0Kx5f0EYVHrD%2FORNYzE%2Bna%2BryW3x292SA0rUCO%2BmWIfgVUcLPuJMtT6nwIpyO02rA9elD4AMHe8wPSCSCxb7PEQm7KkNvelF9IHTkXraNdKC17ZwaqapfkkIRpLWhRm8OgM1FOjugsMKZCYAi7Djl1E0INFDDlaQAWgCULpS0rUR2Mkf5es2RCrsQG%2Brimu4Ph5XEHetUHpUCxQhpgridzWakm%2FDrj2q72U%2FU2MDCDN74uCFQG%2F0s1LS0tOQqNJbV15c6oMJbngMgGOqUBx7dUhBEdO%2F3Cg7RmJw08APL4B8sb9hYOTgIx%2FcN3C3mdYL3qlHq3%2FhH7cgy2pi07Z5eAcSqiUUdsRp0q7pgIJ6JC1GZHRiEt5Yy9GV5uk0NTAqHdusLMkYs5%2F4FlMzTZepo0bEC%2BdFGZDwhNY4yvgaVMWEAF4ZK7mGx3r4f%2FovWOd7KThvzchcY1RgvWHmT4AVU0COXJDX6Ne7Isz3aoCimkPkuF&X-Amz-Signature=25dec59d69222fe54ab722439045f5dabc449c1e6ccebdce017cb0a597ff0e50&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

