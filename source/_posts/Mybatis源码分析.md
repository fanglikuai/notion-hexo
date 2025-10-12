---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665OYLRZZ2%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T110058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIH%2F1TAQL59HzyGUYMRUlheqHEkCfM5SCYImBbhQd2KP6AiEA%2FaQtyoApy479SN0sjJcVH8kBOaxGAcD1%2BtKcwZqOPSIq%2FwMIKxAAGgw2Mzc0MjMxODM4MDUiDA4QwTdZybk7A12aVyrcA3TVeWUYa1OBDb%2FHb3y8c9DGqmOC8f0i3zisQDBZz%2FQ4dhBGd3Phmi%2ByLJXIAZBcfBQQxt0TopLs%2FH2Jvw%2Fnb6pO9HotKXjczTPFQuYFghEKjSsFVpN5xVlCrvWsTb%2Bu4dmVJMvKAWHqVyT8XPYoKWnqb2D%2Fc69ZEw3z7ffDBqpb8zP8xj5ynbUbNb25zb86UT4jHzswAOVlmKgyfQ0LTML5xEA3c3vVXVpR2E8HmNA8s92vqnxoqU%2BKB4J9e7%2BunQ3bGGjnXy3eZamp5SpDp%2BX%2FsRREpMEaEziMnLc%2BV%2BDLFxatbMIjR17gwtG%2BK1k9XgNlN%2FxpYGy3%2BFfCMl4qTBsQPzvKgdc8S1yyBw2w07sHYoLKCLny16mIDTUO2yJ8vL1sJykRJ3X6cS1kJ64C5zoHMitg4KzzZCjcJ2UlOqHvhaUD1NpHcFLWnBzqxvkCO6KbWdXa%2Bkg1dZUSEgiUWqR%2FnZ0wgZia%2FLdnAGDPZMXSdn4HTRJcuTii2jpFvG3IABhOeJwVEoVpqWxdqiNYV6Nn%2F3c1ojlagg2p%2BLBGR2tCnPdVEQ6tlsATqkhcVCO9eibTgGts2hExztnmQslkJXR5Zr6zg9FZnqm0pBInVQ5%2B%2By%2F1zOBXjW2kdrTKMI%2FsrccGOqUBkCMbpyshdUEXhsxH6OOngsJP0u97774q8eFCSBPvrNTBmVGYdGJ%2FZmihWXOBi4tVPMunDADDodBJLTTxIK6CLv2GsvXIE3VCA0dnOM%2BtalyGhlIkVUMl7%2BvFWeaPNLgVoR9P1fPSOyd2H9uR33kmZDr51lfiKrxIm%2F%2F8znsbG2lPWd4mlaX4O2eMnPvInbBjPriHSYL3MhKYCNQancKwIKcqqFiq&X-Amz-Signature=54c35b635e371d2f202c6296d1fe47ece5be76a86b41ba9c611982da0822db12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

