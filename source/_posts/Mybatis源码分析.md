---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2IF6SEO%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T130059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD8psOubAN5pc1s6dN3E2G%2Bqu9mhuuuVV5%2FPTWPHgDwbgIgDH04a%2Bu4xy7byRij7c6tuWBJ6fWGJE0v87%2FXjgZ22EIq%2FwMIdBAAGgw2Mzc0MjMxODM4MDUiDCsvoilSbS3YoG%2B6circA3S56Z%2Bj8n%2F1FS1%2B%2BV4dX83oUMmew9hczxcVkju6ue1%2FuB6DUPZEQ%2BsYpEfkNIy7pKz46zFvQ3awlA9aQqZ7XuSaif%2FGBlnhbbbyBkgq2aseG2GOKD5dgC9kxEjhT14L5K0BmP8R8qVZ70rVhzFcD3EpXLSZGnKXsBSkdQ0E5SN8y5jk8TBF6Pa1S36xiBu2JjNgOe%2FIGKfVlo4zTsrTlo6nUI9eWHIFOUBqHhCm05Ss7ZgaBqNPCFw%2BxRMEW6cl3NG5L2n0ewACPyaKbjV80xKjc%2FGU3iVr%2FSaEBk%2FapUFjsFBJJYoJaFHQxM2E4aZftYkVJTrA2SZjIV4yfPUWxd8Y64emf%2Fw5u88Ksaupbnkq%2F1rdvNQpMaM1hjjzpcyBahjnJJrhS27IvQBFby8J2hf9Hgax9NsiOYFHzNNM42qKs6KfO3yYVxgpcYFfOPZ1Pne3uKTHGg28e6sTJIVV%2BpYNMs2IzHn2jNlyB%2BzFRGMK%2BWVi1hNM6GCw%2B2WPWpGPsr1ATHU%2BoxqPixBtySkMEgKgUhxcHb%2BTUVJULUn27gLuQ%2FAT56v5glzIHrKd5gFOEJuM10LyO4B9QkhVH4%2BUzt9POcpiqWgQpSMG7NuzQLusTIxgNEOi65M51%2FdtMILY8scGOqUB21Rd4qwLxUwCQvbK4KU%2B%2BelvLi8r3P4Qe%2BeJJCStunpb%2F0wl9nc3pA%2B8tuSHME%2FdcX%2FyWi1FgJze50A0sXwrUSDeiVSBFUX01dvcXi6%2Fr7yW48vvTb%2F0QWV7GDJquqX%2Fz6PnnWp3oUs4SFwjMEak5Avxwd6KT9GkEeIgO9tUhohhMi5OY%2F8gbH9jaZRB32lDG4He7vJ8SOozxHoqfF8R43mpEo3h&X-Amz-Signature=32f76e510476f0af8563bc63bf6b99ec925eb8f7c88b8c9c631350c36ffb2f83&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

