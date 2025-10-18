---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46677TP7FRA%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA8aCXVzLXdlc3QtMiJHMEUCIQCeTAJJVaWv9h82G4yajd8YvJqDLvHwI0XFgt7b5xMViQIgLE03DC1kIxOzZUtMdXx8TPZXEf4ucEUpR%2F4qb9%2BHivcqiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAasM4N1vbB1oBAObSrcA4hrL56pcxh1QrtESWycjDAE478yEwD8MYWUIFCyavjvB3825gH5Z4H%2FKRiMiKXvyRU3nhb4BmtbGR77ujzWV5mrS5iVkQYgGKdNGLz3jrCSU80WGSm0d2yGjvX4Bf5rFb%2F%2FFl6F3MumuuPBB7kY9sGla9YyGd%2BJshvvEKGJakfiThChehyzAwfDL5STNoFHtHq0odqYF1HWPLp%2B4n%2BZM5mbLQWxJPls2Wn4%2FWPEE5S9g4S7Xo28hxG0a5dPe7UYfAM%2F3dPJ%2FCt9Vl%2B7WByWMs9fHJiVS7zS8NK0WDfONVNJLZlntTHCFyiHGOlT8gFGZZPeodaHzC1LAv45txolKh4j1NtugwnDPYP3E2OqMO%2F20t8pM8ED5gtTveJMJExHquhL8TPLVzXRcUoFmEifnEouBEHoJK6e40Y7Qu7kGboqxr%2BVJ8YewHcRRtMD0CHIjQOtED85mw7ordckHyv%2FjNTZWucZQ%2FhqFhCNCMHMpe%2FSyPr9YwsHAZQWZDGv6HIDLd4dyjBa3TuAYmdb1egGWwWTwKsKOuFVfhniXrDw9X2FW48hN9f6lUP4v6hWLOcQPUFKDnFSEkI5rd%2FbvN7sCXVsUFcYFc%2FAwSyfTt4E5ByM%2FF6Y5miazc74YaE%2FMOjkzMcGOqUBCpX2E9113GTMlaATD5QflAXLU6a8Guy5d6mH4B8RjiKvl9kizc%2BWU2bVBxvUGZkEIKlGVowoMdRtToI7UERgOr3K4lRmEk%2FCwyXdtjBrfbF1oIhI0NNZXK7Z9LhmSztd%2BqIdNLzXLjIN0r2ZCnx0ic7Wi4EtMsx%2Bj0Ii7QRsgz%2Byl3qirpm9Hhht6qY4Sos487fAeu66hLyuTE%2FvkqYiVpcXV8Rq&X-Amz-Signature=c37b7ea856111199cd91b29ad7ae1aece51092ab6c6cfa969ca276205fae6332&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

