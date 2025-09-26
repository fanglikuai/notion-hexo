---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TXNIA4TK%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCZQCVHVLOxubSEWSdr62e7%2BvydJWfE5oX44n5qZI4DlgIhAKrwBF5zTQvrgT%2F15hfwiRL4ysNjOklCknyZgdhHHLDmKogECIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgycS%2B%2BXwVxvWzXwBK0q3AO%2Brwbs5RcSfMtEif0Bi8AfiGnaBs%2Fzn0l1ZFCubMU3XdBqx1ablKik0o915IcXPdFW4o%2Bqk359KDdi5PlJ7hHVuflX9feMD2eogMNMUY0%2F8pAXgIl9ByK6cJZYbQvSjbf8xG%2F4E5K4bjA4ZwY8vT4zmu7s5VKUgJal1gXDUBfl63J9efnbXmoAlwTk85YDQiP5Fz9%2B2I4vSjWuanerOP1HleaKNcmYbLyPKkRKpCTP3E2%2FkqFa6hUVo6BGGMhutOCnhCoPaJwAduVhEE%2BzYAWcd6i6szmp4EiV61J4Ufpfsn2kAX3UtZBQtDYd5Gf5FX8ponbcU5jAzlgDSdWfwf8YVEOr1U3EKRrbJq0K3WDNHp%2BpAsyHsALwnmo7ae0nu%2FB4KHx4yGL%2FbO%2B%2ByhDK9LMHMa1Pzvmne88dJsJNeYwWPjLTOiSkp94vlI8fPwp%2BpIKxmfh%2BpMhSaJUuCTH53lGSKsVH9vIBCo958G0VM%2FCRXny1Bp%2Fm6SdQTpBEHiSImIUa8Eck6N2w3MbQ%2FTm7wNRy1qte9OUd4auRvh3pfnwxuLk%2FJ1cakEwqbYEWXevRlgNNN0IXo1HYvuV0GPVU6Q5JAw4zv5KJVtIwAjtzoXam5kqBMTWiZNGYvqtm8DCI3tnGBjqkAbaZxmUKn1E2sS%2F%2Fa6yxWqqRui2y9Vv4XLy%2BMuDNXb3X%2BvqR02izg5uDbmX1nWvcfPLJ08iYRuIXhKIfhOTPM1qQkj0%2FLtQ810OkMg%2B7qOaERwtvITUGpqEQSHwWLvsYwMvkmYui7xNTWYOYgYKURzB4y979qy8KLhpyDIbGBod9h9YlKwoRw5VkRYFH194bWiIZktWP7Jp8Lc%2B72gk2vcSXNK0H&X-Amz-Signature=78fa12d0afe9452d4aa50cdac529883f963f87ff4b2b3c443846198125cd2610&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

