---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XDUPIJBR%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T120057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJIMEYCIQDDo5JL7kzfVxAPSsAJZvAlRwUoJ0u1kSnlQqSlrcT1zgIhAIkGLXgVtnXyRzeLekmWvQ5TZef%2BFM0j0be6jfm2yBN0Kv8DCCwQABoMNjM3NDIzMTgzODA1IgxUAr2a52LV39fsJfcq3ANi4CHQBm2CGQ8IxyiWeh%2FeJE0WVTMei5quqSWmM30B52Cg2UqMDdE5GngEi7oE%2FIduiKGarBjdW0KCCASNIH%2BgUxQhB3bSpZ6bk%2FbP64dnkcxRCkhz2uYVTOBXNGEbquY1s0tl1yXMVTNMfAdgP9pr7idAxTcH%2FuEXpIsAc8eAwlJyUur70fidoyfIeofyiBdFz8iiHeApZoBCpqDx6fM4CjUdk4%2B7%2BQo71Cdl%2Br5a5wA5w447E7u2lkt04TiiihYwDkVdlGB%2FxahWcXOuHy9tt1JIxFOQuJjDXg9Zv1T7NcnNwWolBEDxlUtF%2Fg8eP6HBGvcDapeqJBcwgQu2jrAqh3xY1vbsRdFzyFky1Z4DB6GHRPk58V3o2rVXoia9NhVk2WSGcUDW8zBv0bAG2PUTe8wyRChHjSUf7PY0KZeu2giCf9UmxzN%2Bz0A2S5NFSyrfTJu9BTO6aucorDBMvxZiE1%2BFtmRMdydJ6JGjymAK3JsVVK2UVwjGEhbi9p34c%2BnwAdgBDSn472Tk4UGXdrZf2Aj8LZ%2FSE%2B%2FfoR6FPlAlNRui8uUHSAmKz9yD7RHRzGP7eHH7v%2BIAg4JQw81Rf%2FUg7%2FboarjHWevUCKSY40KcwzeRAZ9geWWyDvev1DDk%2FOLHBjqkAYCMCeTD7sf7eyoDEo%2FgsiF%2BYwrfWICGxVOgdpd5r3jKDG85UJ0LpFt8D0CUt332mO75qsPcwG%2FZVA%2FkivEWdUCVEjJ0tgG7FY1TbfWK8Z%2FCkDwuEIuhVFw5bNEJ53g1GR9kbEGHBr2Wtdk9k9PCzT%2FTwkkUGm4fUqX%2FJensMalzfrZdxvb8i5a4IJ04sjKX6iwDTifzi3rtiz58oMOZflyQJ%2FDQ&X-Amz-Signature=c16f611486950b3034ccb58875441c67bef98034c84a3dee8d425dcc5e4eef88&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

