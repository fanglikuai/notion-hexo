---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCHIXZFZ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T040052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICCgdDMmXewBN%2FnJOrPMjh4lVl%2FSYBcr7Sjy8RyOW1%2BaAiEA2Q5xSbMgYNEhCwXYUJcE2hR6yo7fzluX1IG6Ex%2FESRkq%2FwMIbRAAGgw2Mzc0MjMxODM4MDUiDCrkTzq9ZTC0LAb3RyrcA8y9YyQraSwEBUW7xM1%2F5CTrGQC4dl303%2BKQRC1CWf76GgjasutuAPrH0OYv3Yb2HdNCoqXWFUdvLkyb9mil0%2BTcnEvYzCYoY1YBBA8vyK8yzL0lkR5YcA6Sua0SsBhJ9i%2FzUvH9iqbFvjuBaAhp6H29%2FocoXAg5hKUlzJMd0YQRzRjyBmDc%2BhG2UAZiACmrrDj2bpnlrTfLksVziQrsCqUkFOJZZllyyCf61ZOZZ1%2FvfaiMxrIF%2BDYl6ylKzjnhMZyeaqTvC4fQkkuLeYJjOHirkLGwZg%2Fo0Kh3S1u0Aty0OhK5pSxfhCREYR22MtozakNm7Xvb2aYXyMDLoh8v2ulCKCK0sGEdYHWbL2lXkn6%2FBhtmFFl0gTgtFpzlA4kNFJ31WT%2B8TjJ5Ly2vjqXsdMZqSKhtJjuJA1EFriQeofi2y4CdiVU%2B6RS2RRiTCXemv%2FulCYqCBWIejzba%2BcgylhPyePrKrtSXYmLeQwjladMyU5JTgRghmFYV7yo2GC9CGw4M8UWCkhwDq%2BaAMXQFDZKM8uAC92ZTcjnDrP4n1lztE6009pyriVzVSermc74CP6M3aK6eSJv%2BMWKuISamGTo5u3RQ4lexPSpF7pdGPABhPQOpWwgfQwEcZg3LMKLlpcgGOqUB1OIv8a3HFz2cvcoxJofKZ1QYHJARkYS3%2F4uYA%2F5FEoyr28wJcE7Pk46rYaVYbl%2BkLx3ZlfvDcD8ZQw4HEuB6AJaQI4PWEOFvp13zwvslXUih2fbpiS0a5PFMA68LuS3nBQQYwqa%2Fti2et46U7Xh%2FyZ5qoNfkiVkj%2FUymqaBjyniFEWXMq3t%2BiXgUXcdehK999Quc8qUNHj8%2B%2BldMbhGPY1vrHMud&X-Amz-Signature=4fbeb48dd3ae5d4adf4e8c1d02574d7ff69dab812b9d1b57038da6d7f7a77d8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

