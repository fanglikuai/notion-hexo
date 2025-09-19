---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TDBEHZGG%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T110053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFoaCXVzLXdlc3QtMiJIMEYCIQC9iyWSh5C212lYj0PUaJSyjbLdCcRepVyar0Lfj1g2nQIhAJFth0EKRce6fMsZ32omj3ndLATcNTolrm%2FwkuLgCkD5KogECNP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwv4WDwhYkUUWb%2BSCkq3AN%2B%2Fg4feq%2Bp3xp35TeEbEXa0LqY0MGU00WrdEppFSUNAAJuT%2BSCJ6fj2dNG9HGyGTkfX8nHEssAOmfqDvPg5oJxQt0Q4yXNgZ3iYKYwc4rl9zPzjvsHAw%2B5x8j2EnBYtyHQWUhC3C0Q3phz6slxltAodgU7OW%2Bd2F2xFjZT%2BKeNyqkDE2EXE4t1CALje7ySP0oCT6muDb5F%2BJKIn3GzHitbPnwlveVKBB7Ot2O7dJL4vdgp3zZzY%2B%2FCPK9ztZ0axqNIaT5WU%2F%2BxxIg%2FRKmxuH2cMxyd8fWcuzB2vp8d53a7VID%2FBxuI4ZqznlLUd6WtiC%2BVIr%2FLr491e%2F3rSOs0bFEECdrEjn%2F%2FCBMD%2Bku0VmSrkAh7gTNrlmZkvv166CFgAV5Miz5pFuNAW6AxRljUesBhkezdJSt60hoJoxXdbhBI65QgPt74YB0LDInL2oV2gFFBveBkapHqJ639jcXoAqDg12Un%2BXdg0Kvs93hR%2FkZvdY6xiXqh1jlBtEbzxOArFc%2FNMifcmUaWFFKQ6FEHTOvcuvepyyZ5HYSQQPY5rtYWrTUEnEse5v6HJ6YXmgLwKJgdrx0x2wOdBiHypTITjXPoOXWJf2Cwk7IU%2BLGM9D61hAPKgfnp7QfE12CmqjCX0rTGBjqkAYQsYdR0SsH8ksnDJt70mHYCQnT9H8bLTc8gR8pVF9Nrm5uJ2pn5Hx59n5aLnwXWWo%2FgJQe2QWOXegDUt0iCQd8S4kaUbfsntcS2O2hwBfifalIGmfGRHUMG8hcHXGhmPpsqheL%2F5vWnL%2FSowg1C7Pksbvx5xuacUdE7qjv6Ik27P6Tg7%2FCBXaLAKBZG0g9LKLKcAi6ha9CMFPCfw%2BFfiRZvb9Z2&X-Amz-Signature=35c590f08024098611d641b18e2f01727c3a0419c2669578debd8203bb968a69&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

