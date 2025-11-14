---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SXX4BKTM%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T090038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBWFcPQ5LOVfTuUlKrfBeBi2oisKVxlrLIVHGavxEsMKAiB34AE0MooN27mK3JHs1KgpkPeOvvRnakHPd5yPc79BOSr%2FAwhhEAAaDDYzNzQyMzE4MzgwNSIMfmBrK4voykZl3%2FgLKtwDB%2BXJEwgHSLiv1yv1x09G%2B6ZoILwz6SUs9uxfzPB2Ulc8NmlX6K4GUTAfArXk7ObV2PsEw4gyHWjcGIvTsX43ZD50wIm4dE4duWMtx4V%2FzhrLUR1KxEGgE5oM9X2CrNIHqKBLKI0EoBeas1po5OgZS8zWhZ%2B5ght%2B%2Fi1HKTOvhKg0OTsxO%2BWacv885Lw%2B%2B6cUU3YPo%2FLp717%2B2JRQ41czvVrvWSWQlQRKNsNBUEBWooNOl22UdOVW3RB5jnBGaTUNW0LAUf6i2IwoSz0lMQRXWS%2BSRK4ZwTJznkpd8iO0l2eYdHJdtCWWAQRYdm0AxlzFjrhBlHhDVYHEvCjwj8D%2FhOqYkX5YxUd4qyF%2FbayS%2BYtqmAK99ptoepqifOcObbc1x%2FRCvzGOGMT%2F2unKiFd%2F639rXXTWH0aDosuPPEOxZHmaqH%2Bjscs3c8OJaMAMHexihIiipPK0O9JFDOsuCLLFzBSuLtOUYaaM%2BQX6Hey%2FBIym5jxg6aSw8wW6eyARP%2BgEP7Twk6GvE2EItTmq1h7tN82IUvVIozGhybez9HgrwfflBqRewPmHLyx7hdTDkTB4U4UV7P0pd8lxELXYCZGVmZt14WMly5T5q91ZPcrjCgkZ1DfaX2hn7LAkNagw58HbyAY6pgFfvMMjkudi5qS6mOwx%2F93XSmLj%2B%2FuQxReA6de51o%2B9Mo0K3YvGU7EEn4u1f9fcco2mJGPEK04xAqJGy6DeP12lywAVW7yzNW440ljBKBrf%2F9tSHpsPwrnAqvVCv8ZaPsEag7%2FjG0fVE%2FlT4iR3j5w9ZugRqerGeXp9TmmftxN0TFE0yZZqNJl9MRBLJ7bHwrswrY8y4i3EujvvJwwO89Ptf3D7nOiV&X-Amz-Signature=9fa3cd70613e751e05f4ceead9b88a7e3b7cf374bb07ba760950e985e9331d67&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

