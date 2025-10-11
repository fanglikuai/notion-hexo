---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T3P3N2YX%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJGMEQCIDFBxBnRBlIb%2Fa0dAjevNHImZQzGpzn7aF1385Sl8khlAiBNdFPQ25XIwtGi28YMUeAjEc6z3%2BdUY0xSznr722E6zSr%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIM9gAn79qFL0RCcYvaKtwDuCoPM3uCD6fxQ1qM7oUOQCIdMfqNqzWKejvw6g%2B2gUd5k3t3SvMbZ9P4jr12rrMUQx%2Fp%2FVr8MLV%2FT7LEp35Fngs58Q7djSkpwcFbgvwFimzBpngvxlvKUFehNiA19q1VTstEGgdgyxLYh8GuiH2JwHXLiCMytKFiphlWnz1IOMAtXwknl%2FvCUzHokXRAXCXFBF%2B0IXmkB0yDqnVfOtVhgkNKMPQXExbREjlo5jnd6V4qxprzD0sqOjfF8vYO5uM%2Fbi2liAzP6o8JrkZ48m5lFcFj57uPalwyzSNWvxz2HtDAuMAadGT%2BZrWaebzzy3Y5XUOnDPCkvG%2FprjXdbi4fOJefGbx8Tiz3exsnJIPC%2FPhCqdTWSUvYk5DtYhKSKcsIvvmIK2gNTRtQk9p19raLCdn%2BnDkRLhr0PNalOoZkF4vgo7P9jmbn0xtAlGvf7D0%2B7oGg7jlZQYQeR2F%2FlDfi29YI7kZuoqa11UL4oyNClyBO%2B7wtGIkcDO7Ct7%2B3UBs8AbA65YlV%2BfhCZiLbOFyp%2Bfxmds8pL3I3YDQGIk8lFQkmXgq5VAlgBdstGydbJZCyrn3hQ%2BxZIQ23HdcP3xRHp2tvfvXZ7BejgJH%2FgVnKy9l6owODKczDU8XRedsw%2F6SpxwY6pgHq5mIes30FAuVkrQFnknBo8azKan4c85JwHsguCMeEQ2vOhrIrxp1Kju0%2FxhVlqat9OT9nYXX%2Fj60uTIaUwhll3bow1YiKmm%2F9gWa3apSyO2s02W0MZ6lR8soi3kSJddZkxBPo3KeorHVXJq4Zx1%2B1u04R22KqqCeLB9e23PU%2FFM18ZIJNc5mtfBgfB4DEBxI0TO740Lh4ZcN%2BfI4CKq10dNboO87T&X-Amz-Signature=b9614507cec1910c7bd74460b9e7cfd78198940c2d96baecfe7d624057c3e382&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

