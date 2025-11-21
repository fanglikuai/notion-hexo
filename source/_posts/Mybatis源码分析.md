---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZNPQCPLK%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T140117Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEYaCXVzLXdlc3QtMiJHMEUCIFfBeOt4gU8wL3TEY7MtyF3Z1FMwrwT4zqykjXEpd7D2AiEAjI0YwN0uMb9awZs3ZhTVjBGGtVTTvPisecyB3albpFsq%2FwMIDxAAGgw2Mzc0MjMxODM4MDUiDMPpE9Lo6Y5NywoUXircA6kXaTCFd9E0EX47Uz7dn6va%2F7A%2B5PzhMxrkfN34Q91ZPAgHeB9cMJA6XaqWm7u5qO48oFQttiIAnGsA3FI%2BpSRvq7Pl%2F3aBhCBVM3SK87SjE9DuOh5HLKB94eKvEnJYh4BYuGHUjMxYG1aeebbgNXqKYHKQLmAj1YFqua%2BSI3dxRPoRrQw1%2Bn6zigd801I%2B4B6YLDN2xk4ty19xp9tXWya5K2%2Fj7LhTRbHZAWqpgayBSF4IWGrm39tPLk5rH%2B%2Bev5Y%2FTGuK6syaGNKEUqY9%2F33Rg1olNL9yfu%2FlfbD1yUp6NmTycLbkJ%2FwGSSrGuGyyOrdotXPhWn%2BUg3FoRILn66AM3Zq9XgRs%2BOS3pCH6VkWhbqHGCodcrWAdWPChAAX%2BMuzvSmCY5UyBAMikWYrRYwQBcNYdnERJFkiIIcB3rUZROqGUa2ZJGL23KsPb5G7WL1K%2BhzIcGTC6HNqiwKGaoEMnavxW9%2FHcbLKcK40VnqXE7%2BNGZTZYUE2toGPeXEFCbj7u2hSkmOefV0SzRarYjCaFZiLHuexvYaK0606jDML3SbGOHQhzY1xFQM%2FqyAz3Z3JsRfpiPBdzVrP36jtIDRjXkCov3q5dg2pSlkCq66mNDQvwb%2FsE0Tzgb078MM3QgckGOqUBWG2KtFI2LepEgnVl2hIv3oZgJwWBmzrOO3%2Fl1axqk9UApwVamrFCnWXnXktrBCPb04b3IhAUl8kkyYvR5rE%2BC1PambVqXxcn%2BNF5VyMXkasYwgSyV0Gf%2B3FnqQYToVLVDw9Q2fJJ9olJ9Nw1e8hYIBCL9AboYSv7KnLca%2FE3SRYJLbFoiRN05FY%2FeZgXe6Z%2BaIQWiU6LtGIgFINXWiA6bbpCU3oG&X-Amz-Signature=a9d394cb817928a12451f6a636fdc6d65329416cdc976b661dbac50dabf55200&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

