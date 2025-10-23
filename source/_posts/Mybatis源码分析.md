---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZATGA6UY%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFR3h0HgHbvG%2BSscKLmxzuHF3zJ6WKQ7Ss4cDmwQyCkuAiEAl6gTRkNenVFjtNQ2kEJ2unbfYXD%2FbgMVDZ2YgnPDnAMq%2FwMIPhAAGgw2Mzc0MjMxODM4MDUiDOgy6z0OlS8ycrE4%2FyrcA30jZUJG6E%2Fz7sWWu0gneEkXbTGnCzrhlAwfKVXd9wsjUE0HnuMtKdDPSf14WSwT2jWFLz5nbWmLWL%2Fhb8PNo1cXj21ZkKBhhnSyzDitbD2MmHd5%2BKR0LFgmFg%2BvxLrPn7V0OkB%2BFLpKyIwQ0%2FyleZY%2FWOVv%2Bgf81EVVc3XzokhTuKoEW2E1TKpNOO3fA%2BX4nv7HA6X%2FCXLJRRlgEy8o4IF3RaWLiYnG8GkZWd1v0Sck38pYeGSN0zD8TfpfC7S8KXq2H8uIrf4GbIm6emN150rqe8EwalhUFmxbC4FpMO%2B4rmQq7ONKziW4FjXImNdpp5zFf0049uVfn3sNa6yymzqDj4WTGqj2VRmwNBefCCSj5dhiYOn3hDdtOVswvhSyC0bFO4FuBygWYzfjaUPeMqDMVZzRZo%2F1pYF66Zz%2FG%2BNHLMseRRnFslL6Ku5c%2F1c3yDHUnJhgIt1RtfM3tCKG%2BizKC5h%2F68v7HeKOJ8wkMb9EZkjeuqgOZwTEffUNZ69RB0g2osB5a7%2BYIPwICYfb1KfQgz%2B9RFRTO5CYof0LHoy40zJ5f5M5%2BbH6Lr6TPSFUtW1QYHae1QrjVaAi3L%2BWyKhzy4ryFWveyeROTW8epZu%2F1VSI5GMlqZfLd6QLMOje5scGOqUBa46g3AWD4V4FYJTBxwtm03jr9SygBKxXUs5K%2BJzZYYPMuY6O9D6mTjehACIwH3xu%2BmcqCzpFDVpEfheUGioMrlA2X6jdbfUHtfy2OuM66xnDD52HPB8Fgm3PcsixqjiC%2FCARhHhuSImtoJd5aG4ufdO7anetMgrSAdFF%2F5S3%2FAaaymktBOPlwqL2HsoBLhNn6o2MuT8p50zBMl3yEUjubLaPq9mR&X-Amz-Signature=9fac2768c576f5a5c0557e048b5ab84b94eac692eb8a3552befa1a972472195f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

