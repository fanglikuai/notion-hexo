---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UP4ROHYB%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T200037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBMaCXVzLXdlc3QtMiJIMEYCIQDO2PrWiq8cMe7rE5YiUOWU%2FyGejWDhKIuVG3F4DZo6EgIhAKpx6o1Ap4748%2BUkC2hWx1CdQMYOVVP7t2hFdMvijznsKogECNz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwQuA2IqacGAYIx4iQq3AN2xKoOuVH3eIFfBd1l0vre%2FKxa2vPrQTlbPR1ItXEyAZsPFDu11BZBpqKgvIlcHSPZm%2BdSwKOb4UI4I56eCE1Z%2BcuWB460LSzEa8LlNPx6hDF7zF08ZgpCf7QuSUMK8gni%2Fr5BObXKLudw95qzFKQEXQxaN4N9%2Fkf6rDjjjPNmJiJh2uncDcZXJ49bm%2Fq8owm4qz4LT0THACJbs2%2F1JHjDgDAWKUQJd3sfAgCHtTZSH7fFZKWHCfOAmqnRzgOfzSVjOcPkuDtr2Q%2BqbGZzhb8rlj1myQ%2B%2FVJP2POrEmeOuPZd7WSaBzOT1nQWqIfi2MHLgebznyJFWpmAbyFSE99YdMD4adnrnmPIh8sGgZF7dAkny3tKFqycyzwHp1bRvqe7Sp1Ap6I6Hun%2BanMtpb40qIHzFyXiVW9woih9ruT56aMJZbjnhEmacNrPJ28zeRM2aR0yq69YkA7rWN1XsIWWctZu3GJROQTeeYotoSkldMPJ2uE9Ht6VpxjyR0YOuIVASzE0GVudcuOG1LjDAc%2B1feR9MfZ2TmCnPvDhBb0oiH1sbFgIkS0ejQG2aXUMsRjHeMzU2DwS%2Bc0806qSfrQh381Hl6lVPG9ccbWtC38hl9j0bHLre4%2F33niAZDjCSmr7IBjqkAWG1I9LXciKqlFHpt1Q9%2FXapRkXrDGcjnNhx%2F%2B3MegMjZqKBaEcaGQt5yrwL0IZ8h%2FB7z%2BEJEzRm6uOoQDDlzq4t%2FVuVKEWCqwg9LliRwI4CMRCS73ZVa43ob2Xelwk9nPhicy%2Fyl%2F5yNndaEDjhU7xrtMAW6smkS%2BMXA%2F46Vsie8aWm1lUijNtr3M%2BGsW%2FknKcRGNLSMoI716USkX4hyipLBcOt&X-Amz-Signature=aa70326a26d7fc07cec3d1fadc01a4660d0e516199aa9e424699a98d0a4e5ab9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

