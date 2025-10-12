---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZPR3YWJD%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAURoBQpGw947Ch%2FDIoy%2FOWf6dCAm94Ntze4aIs5oHk9AiEAgKU0tsT0aO%2FUO5WeoEx6%2FwtZSfJLxgJGRvz3uSRvj5Yq%2FwMINBAAGgw2Mzc0MjMxODM4MDUiDMjZHQH49ORtRJG8VyrcA7cLEURwiHa2O%2FADBRAWVhyFqNPg25lBCKA5QPvQZX1Iw%2Fm4IAM%2BJWOu4ECekzBkMdx8hLyXGIzy2p3wmemWjO8dFTY%2FBtA3cI9%2BaA0rpsXtsTMegNUqzz4wXwQCWmwKNc%2FaqKuXsZ7FITJ%2BJpYlYn7z8Tn8HWUr4FfXZnLNsOE2jAb%2FSaSI08BIl7YOO7NXEEeJsmElkEbYzru%2FZkrpkSNDWR940jXDMje3ByyoMcrJ3ql8WUVgXrLBkq5WtMb6Ps%2FFMsyk7ghHGDC98GrBALRWkc1sS%2BELNIwzmRsjlaKWbQJmYfkkJJCkl0pkg3y1JoWs5Yb%2FEIa%2F25%2BQJ9ksS8QvmFdYaAp2jlqZvhc18cIflwJh4elXGBVjrOGyO8kVbdSgobS1sM6jKwRnJOYs%2BqLyXkPSO97Xh5VUGgY8IaxgEZJ7rhu8l%2BtMK5C878pUcOmWej%2Fnydt9PaiXuPUxoV78WUfq%2B3rf%2F6fmwJe4lQtGkq6hp9EOuW6UjVOYyrDMQkBDfPrpbnbnBVu02D4YfeBWzYCyiaTJj4W6MCUGYdpUA3Nj9Qg%2BJJQ5JXjJX818Ll%2FBNoNAViYM%2BNGxP33tszcwSv3IlJ2USWb3%2F%2FvKVeYc3WV6%2FLdXqX6O6sW1MP%2Fqr8cGOqUBFOrn3Z7G1jCdC3fE5NiAPUB5AK4JA8G398UEXVxNxlIe90uiimf36HBr%2BlY%2BxdvEmxtt%2Bm02TGoa2wB8IlQYId0fOQXOnx%2F7etV%2Bld9uWnfKONcsnSC%2BkKU4q98tzT9eOPme2dCDB1LCCA9zlMHpk7wXxbjyiyX4B0eOtWg5aYqujdGA0%2FDFBUm4%2F1f8dNoufDoQ%2FKTDaMsfDASYb1yedNgHKwiH&X-Amz-Signature=9c736c1c64420ecc2a6c5a096575b79cfbbbfd76a432bd5362307a16f20fbfe2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

