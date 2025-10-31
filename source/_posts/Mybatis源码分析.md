---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SG6DYVK4%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T080040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEgaCXVzLXdlc3QtMiJIMEYCIQCU0WKfi3v8KiLLndl5aodNJ0Q3r1z%2FFlNJ0e2jGBUn7AIhAKARVAI3u%2FwrzVhRWdTW3Zmf36hRtZO6Gk011V8xo3BNKv8DCBEQABoMNjM3NDIzMTgzODA1IgyE7I%2B1AOuHkb7LBbIq3ANJ%2Flb2kJs%2FSw3Gev6mtWc2fUKaa4VUEeSu5etAhE1%2BquYEc5tVIAPXu6hCIATUU17oZRmryJ1gvsyVBuxOZ8tzgGacqn4xsciaZoG8lZsdqP%2FAEN8Wm4Ym%2B2JK9ErzWndrmVtIuMrGCabsflZxL5K%2FjCbjyJAU3t%2Blc7Q71OEimOBw6lG77WoU%2FfCJtzCyhK8Gn20qgyM9YTbyMUlMX2sHcDPlzI04JpM4OIvyH1aLivqC%2F6Qc%2BAtKf3%2BRKnejvRht2etBedwa23gHmUxyepglbCl8srYwkHD%2FQuc2U0DJ%2FAW4G0n89%2FPz9CAe0jyfQJn7nkkRVlOidNYSqLWf06Ue9jPMe7FnKLzWCmbAwnDLjcY7WzjpqjT3eR75phV%2FgwF6roNhQFJl8TsIrZxa%2BTXc4tI4FDuVU6nrcHOY6sTeovOMXoooP0DhUkVKuPbWt5aFHp46%2BficF4r5gy96qi6O7ip4EmV5MPIsYdFStGsLFuAgap06QUUrauHZ18iRd4HN5WU8PmuoxpQ7sgZxBFEurCB61rthOgzzBF%2BFYc0tsnLqwWxQ3rC%2B7U%2BGtdDe%2FWq0HdgkDxGZY2APOd28hs6ZYYoysSki9ecgeOer1ytkhQ%2BJP%2FOG3GK2tKn59zD3x5HIBjqkAQeao9WJhrlNCrlmzmpBqExJLE1FHI5c6RWdPmh1AK3SDkv93f7DnFSbV4faxENSNfs8b52dgKXexgdMkBeKHq%2FXElrJpxwpXOKKSjfNS2SFbcyELxJNs47RRLSvsBKmzm%2Bb60gYzSp%2FeK5wwrlh5K3OO6WoPmffvjIYwHq3iSZn2d80%2BLJaF30gvOT3xdylv1s5DR3xS2LcJzyl%2B21PDlHE4TNA&X-Amz-Signature=d8c19f928feca071614b7dcafebcacccd4b4091a83e0e1adb4f3547dbf51fbfe&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

