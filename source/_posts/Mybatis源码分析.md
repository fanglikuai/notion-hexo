---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZL5FL477%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDHAo1GvhdhbkL3mwgMrBl9V2UsqWct%2BwnMsZTGTdPiBAiAXHN%2BXzRQRSgvptoQhPJJWZx9vhToYWXtaob7pKyYuKyqIBAis%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMhUKWIPAI3jRVOa68KtwDJDAsRopVAwFr7b36K4ewU0CJK2k87CQ7zPpvp%2FUR3%2FwriqERlQM4N9NLXoq7zpzm0j9uLZ9v8lOapBn6%2BZvY%2FnPwx7cfxBHqYWQAds7P6SulBB1337iuwMVMw4Zl%2FB%2FwdqXR6%2F7qht%2BO2XBKku8LIc0vJrkIMgY%2F56GjDJ1lt3F4HKKZ6f2vP3g9b98Ly11Klfmg1%2FbcIjOa0BCvtcarlkSkDQ3C1F0YygC%2FUv0t8pPTS%2FYHd2gf%2FyrxoVpV9QxxYD1IeKO5APKg20w46Reo7z62A7VXKkx2o4oS6XbqyTq6sUPa5gAB%2BXGZlZy%2B1paTVipPAvON90d2VU%2BGiCBT%2F6J13gykI%2FdrgpwGO5CAdIuU5Y3qjti7PjuEEcAQjXplrtNC7smWm3dfwv6M1tKngbfwWtHefmiR0mwzReh3k3hkPuGsWbukEfCMMrkuoaVfPR9D6NWL7iegpozcilG3It6RLMMWn7zWN4I8q10jSNwHjoPKq0pVxUgqkVAZBErTKnSTrYIerCM%2FdXgeJbiR1PK%2B%2Fx6VxEo91TGVLFrfHY%2BV3DIU9tFv2%2BGrIf0vtwNIbo1CHSuqw4rqkfnvXnfr%2FkfYzjIXmFT4P1Q6hRf9wcp4HhUZaFjFCzJ%2BuUAwuuSzyAY6pgGH4aeaUWXlcOfp2S0T4XDQ1%2FJKBs%2BbOPe6naiRflzdKMu9DHbYo2wvC7ePXNK5RSQz1YVsCQIaelAWNT0wuQYfTnfikBN3dUpcYVzLryzE2O5IVULlR0aegKeuQGlk8RMpVbESAQ31u6pN0Q9TM7AqZZWQJacmyLzFlgWprxoZWVUfBtSZIym1EXWZexdSygWaqpuUN8x5Yw8tSmdkWxegqO4KX3Fq&X-Amz-Signature=36b95adcda73d49b486f920287373e0e3ca9acd46b314d34e5000085648ddc99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

