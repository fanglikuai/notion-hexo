---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVXRJI4Q%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T080255Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDWKxjJ3DKfjV44mG1cMeIZDLAnxzGwz2F8BXUwQZ1EkgIgF0SzDE%2BATVu8E5K0ak1RaJe6rIwsdsyMVrGviI7Ccv8q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDGFH%2B%2Bb8maMhvSavdyrcA9WAVsfaW2BEXU51g3E0Uk8G5gVnJvephcQZMG0E8rWSVgd3CRgPJwg5GBcnNSdWdosTXxFkXNSKighkbGQ%2BYvfDOEmUbqvdguIS8aA7W0BXSFWEoQZmZdasWi7V0pff7uqnD1qONMyRRHNFpjfU1FgfPF7sPkoXGHgQQB6HXYxmReJh1sWV70o6W1gfLOfK7cxvv%2FIlR2DVMJyFThUfVeRGivQooG98xlJVBzz0Aw2vXmKDdDaei%2Bzd70qJhgSBAdWX1bJ5km9E3wkdAZZX2F42f9KNZBGtVCYZiOHtCav3IzTp4Drf5FPGSfY8bsf%2BT2s7ZvoLMB%2FZNpBLGG40w6vBu8W8A4hUADOHcwKNeyKFkYrytxT0ogcZdDmwZt%2FFTJX%2Bcm0GzEiynOw8GHZdGAE39q7xmzP0HQOdX1bLLUv%2F3F5QIG7gTbCHri0A7HyjqC6ELo7rJuR7%2FuDru1Ur%2FMoA%2Fg5zF7Xg4AnyGyaYxBee%2B2APmt8lBrJhWCGBYxvkSyRwwKtfyaf2LfQsXGiGRf%2BOwmY4xDir9cYKxyXk4x%2BK%2FhESd9lpDFfM8H5waUNjJnsz0pAddme93esHwMtexVf5yertMvHQ2trtxVRj5KA2kK5s0jboqGtWQ9KLMIzP7McGOqUBd2khd38P8PLfqvtE7zGiuoNRil1R8grx%2FApjg%2FhSndsaz12AmkAZzQF3nLQ5gJ4sYEWXNvtY655GznH7dqfbcdkWSvJyPrE7ev3ICGef2J4f5cQn%2FHSTqgNCFj6jfFEq6GpUV%2Ba1QDXMkCaLTb8ilzNOzvfV1KvP9otX7UrJ3H%2FCpgJE2tXIISqgerg5JQPRpfaVB9KRihJjLszSY05fKlGL%2FuI6&X-Amz-Signature=d082f62ae37e6873f43a209e36ad39657e0d2148c5282f735f1ce55c1e00642b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

