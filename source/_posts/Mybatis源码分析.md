---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUGYHPMT%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIBi3XFLOiZhkv8ziLbyC2hC0mTFWQQuKlkxpaCb9SI2UAiEA8zIvKRCFYV7hBQulrq9YIsZrHFs46dwF4WJHWTEFwh8qiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMK%2Fa9oHQ%2FChpeaqlircA8XOWG7My4lgON4%2FtYGh0sQnQAsAZvX1ym3LlDkEAEWORadfigFDHm4bJrY7FEmXnjMGGdphLX9rh3wK1zKAcBYqNegCPVhESZ9C%2FaoVqCzGtrNb4ZHPX%2F4jx8ccg6L7ptF7mnVCBtS%2BgFwRYtbw%2BNBGenyAmbAYEX3qNdWrJymAQFBErV%2Fx3DS20yFGIkH3HJfgSMtCDQJuT8gV11jHXEQp9zmCRHmWH9Bkn8H72ItPCkVQOCvon1iBj%2Fl0w29jVq4hj7oUoARE5Gpuipt1%2FGUpPOXMDmeEUU5R43cZEz29XJTvIkL6DlysHRtGKpFO0oLnCTulGv6dWpq2HEn%2BxJNEKN7VJkM9xZeE8hjalpfzm3wP3H15LU7ClJDDDgvxKKzusSI1LqgorFaIQzJZd9zaczakbniYpEKjyitoiEPiOVvaBw81TjU5TQMh0JqR0F7ESy0FcI8Q09vlpzCc8NT%2BEIjU%2FLVta81FIxfV7yMM10licr5ISny5%2Ba1elJU4i6Fdk2zOgEP75NadbIDrNSo7FKKNrCeWrAesiToFnQhiOlKXP4NCmx01lcsFJXkNjsbq24iK5VgOjGU1YSwmoVrcYJvXduDbytg2cYMa4NL%2FPvaX376Nk9%2BBD4f9MKXZ2McGOqUB0qQ6Z3nEfhnSRlCmWNgvGBjJq%2FCOOUFrHKpRkBJZFlIL1yfAPeF%2FC4N%2BpsHkPuEZagY7N0snF2fIyYvwz8PuPXprup433N2Lejhz7mllAaH5p2tiRV9nt%2BtYtIqYnOIbZmRK07HNYRvKXhBz3C4VIbnZQViopzV9qLfTyXZjc0XkTnh0oNBOeoeX3pZ8vkccFxHhTEn%2FSAyhvac31pUH04mBtr%2Br&X-Amz-Signature=9a2cf12c4a9ed490f3838d453f4076ef58f7787937fa9545f6f2ad33859e8ed1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

