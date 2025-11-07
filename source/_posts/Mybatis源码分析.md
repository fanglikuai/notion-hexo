---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667BPU7FXB%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T030234Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDNiWTuOdNc3kmHRsvBcvTO8XBS6GAEsPRK12ykvEMbSQIgNJyIcrPiPt4C7%2BEIIDxIw3GqUK5GvdCnULdSPVTN00EqiAQItP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJ800jYwqoNzm3g7RircA7%2Fp0V0zAf4tZX8tpDzD29F2NRzPdY4GRS0icig2OJIKVZGR9bj3YANtf2mkAecAN2dpbgPqki2jOKgZgOHldzmF2zY7izPHM%2FK49ZttqaY%2FmMKPyLomRN8NI%2B9%2BbARWXXPUHVE1DtGXeOt691K45nsREUoeFFQ7GCyuR%2BN6yV1lM%2FZI0mcCmQRObjwXjudC46iwpMdvjDRHpQjEhMnZL3YyMr7h5gKB3Qf%2FldC1nmGwxvTYy%2F7K%2Fe72K0Ni51Rs%2BYHahO8tW%2B%2BcJRWTSOhsxM4FGK2AojzcQaghaOwixt0AreSbWm8zQV%2BVkeGZi2bckzpUT3Gm9NhI9dCz%2BoJPzkdEVdQKzdDv8Qxy5UAdtjRUZlmw0utJudGyexs%2FbLnW3pQfKzpLJ2ljl6zkkwPsJpf111eHOYAUpNZZih%2B0B7jBh5UlQneJCWZigtFdEX7wmjZWzhJpsgiV6y15gVp5QDYH8B8cDQfNaCGzbXF7wkVWLuqPuvEblo0g6%2BIceyvBmZKrQvYlOqyJrH12w0ligWjkNh0DdFMUG9VSs%2BCGZxtNOQBmFJ0TnGS8TWCapdit4b0GG9vlq0Pwx5lK%2BeN2cM2U%2FSbo0E1w46GKEw0cBAEKv2L1z4vRdbQtPrOXMLy%2BtcgGOqUBsuR2LKLvWB3Bxi9cIxQzOOHrpcceDBp3PoONK4zI6bzeVhFxFtOZbXUsitKsTDamq7gHoqM2W5LHrNQfrAwk8RIRxMpeAxVkb40Fn0M2xZFbeEC74HYQ7yWRRcU2LqJz2H3xc3PCV8PJ8ewJVS4mZ5sPVXlSlrWQeuEGxZ%2FXAr3JJtaCpPELN%2BTKDbRtTB5GtLbRQL3qj9dhJDfY2Zf5jC1p4IF8&X-Amz-Signature=97d7034212c7b305e3fc9fe1075e06316fb7b6cc75ff240936ac8b0d7a0e324d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

