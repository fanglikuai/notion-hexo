---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXPIUOXY%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCgiMnUogbzAOHsuAvbty6Rwg1aIo68zPlKWzKX8%2BAk4gIgRBuwrO0y7eIeL2w6hOR0Rk1ML4nn0fVVPWCBFyv2Bscq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDH7DK%2BhFpQeX1vZoASrcA%2FwS14W23a4wIFzt1nitQXDu9UqDCCVlg2LsRUZ3iPDkLqQB5ZeVb8Ov6BiJJZFpnpdNhMYhNN5f8s%2F6yWHtw7Cb3eN2cuSSjC6iGCbjifge3YqHT%2FAwkuwWqxqU0tYNBNtMzc5KkMbyuwjIyffXH%2FwSnPTh%2F899HT2vO9iWdiOCpuHgnUG7a8ykNHpSVPUtVLqCvSNRIcon8O1G4fmlbUyLJEbwcvg%2Fr9V%2BAE1Kb5MQN%2Fugw2iPkHDrnrl%2FiuKx59KtGbr4T7%2FT6XsWwXKoaUoX3V2PoJziZiHnniqVRBAQQYO3O09Elm%2BvbmbUF9HvqVFvVSOyc1rPbZlrAJ2yiSFuSXcc7UbxrZqyjj6Kz3drOLXdN1v4IEmUJfp%2FVPqwPBCgk1i00%2BTJq6DUiFb3VaHaYtL8UQ2zFZkB3wEguzUFKJP1kENzPChGMCu5MZl11bgbAzqhE8%2B4u3Zkmd12MUHUfEB4IBJASZYYIqncOWpbFiiASBE5xoUSaRcGj2BIGaGzwUKvlRbxqowazL0YdOpKl7UTZ2rz5vWvSTRCtXx2ykFPk%2BBxsR%2BnzuI%2F%2B5O5MSA5JwZXCl%2F%2BHi8z8o6kwa3K6%2B9DLpRTOqcLKL2FxWOWOVzyPndg7uGTHrwZMO7hhscGOqUBpZ6wR6FVk9HnntZX2Mtkxcl87JzyTFGZiT45mWe4LQhzd4x%2FmzmlosFqqm9%2FhnC8LoeJWgiZiHr4Kfxf8LUf5WWZgttC2gM69TJTTpoXUBH848j1q%2BZVJbEIZNrbdIEQLM3NFZDNR3IlWQTEvI32gB5moPqx8Ugwuff4GuDlIHRL3%2FhoBjbm6DPjgfrEsOfXv5KJ4o6%2FdM3WiUM6fzk4tnMu3Eo0&X-Amz-Signature=fe637bf73b4283c16756aa78b4c7c84136b2c87731c238a42c21f7c67f5920eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

