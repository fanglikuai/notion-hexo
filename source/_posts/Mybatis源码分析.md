---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OF3KFTI%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDOvfYfuVKNq8kG%2BPNKM87UG5mZQRoQBNDuk%2FZf7M1qPQIhAKFFak%2B5mzt0FTgKve%2F%2BpfOXdCaLYMvAj3ICNAvt7SOTKv8DCDcQABoMNjM3NDIzMTgzODA1IgzXzIRElZWZK%2FSBGhQq3AP5Itz3OwzBAXLItz5QbeY9HOb717%2FXs4%2BHgtpqEOsloW0yw6sRm7wyU4AIvId6Bg1PZuqCNBiH6jQKQG93wFFSCod1a3yvr9xuriu5Ksa2ym0792FNKFY%2FZgvtPoT%2BCeZjs47DIsv3ekCocmOI%2F5eB%2FlnuRWszYlDmgr27kUy33z5jWu3oHiITfPJHFh1ooLEr3NJalEKpUj7c0quDq3Gfxg1u5LFeL%2Ffu2mOG6amYdouP9n22QH3UXB1Br1u5zUnZEAIa4Z6%2F%2BzxO%2FsB1I1Zd9igRK%2FRMgyd2peZDP379%2BcwLHkbw05PN3kLyZLiDwe%2BQ%2FbP6ZmAOA8%2BVfChR9PdpvabwKrED7w1EYNY%2FRCoE2JIDgfHWj9rWO6sje6nIeULH%2FT0Y2STMtKa1HnvmqG2ozMiSWKWDo%2BNYUMphm3KQnoiWBq9OCDyVLM8JJw6crK3JVvSCfncCRhCeTA5AePCDRAQkQJhFq4VoIGra7GQvfoeGvho1zUEUTLSN86MdAzZPkdi4DGnsGAVIQ1vpBMzTHNu8lxICoSUz5okyWk3qvUkGTRhOnKjPCQaqThURAwG3YfLm%2FM9MJhRTrU%2BBh5lPiPKVdOVQB6OKwParHL7RFBnadsMT0DThm5ylrjDT2vvGBjqkAQXu85oU%2BtyYILAqfU7Sd5Ai4pC3%2BEa1IkwKGVqr5tNGqH43Mp8guSKQWHgVZBVd7f4XfC5wn7R6zSkyYKVsY9gNeOSlmWQN7GR4ldDFD2FFlC79FHuIT%2Bclmq2xE9OnBVT%2FCJBTPramRw5eMEgopQSqvUiuGoDvl9Td%2BjMMt3ipkETYYeNkMUWGxaUTb9Ku7BqMYza15dhmNbS1Cyu%2BYYXyq3Xw&X-Amz-Signature=3e6d039bd87b82ffd88d44ee43be8b8f3d412ebfc9a2d5ea539c6ac9e5bc4f7b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

