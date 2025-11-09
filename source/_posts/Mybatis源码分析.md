---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XTXQIXOW%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T080055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJHMEUCIEljmx8e%2BCRu%2FasRqfasYyyq7Tu5HYMWo6WHVIz9HGU9AiEA%2Bd6cnsDQBpnMKzRk4YqGrn8Ap3BazSd%2BnKNF69pkys8qiAQI4%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH7tj3adw1SofiChZyrcA2iYmxuguq%2BrIdohUWD5upAuUdXy7rQj7oXkU8RHVyYoTVf0%2FRGXMr7tPXGUfQJAabObFpMBEToF3jYXNAQ89O8Y0Tpt1YFQ3xhg4mit6viPjzfgYhjsTTxIa3rIY7Yj3HT8pQvbWWWF0we4poGcF5mdLduZxmH4acznWOnXXB2v3X2yBniV3bm3vOMJrqeKGkCYItIIFhbnjsevYP%2FNylUiuIqwk0mrOm9q2gpKE7Ig%2BdKnzKqI227%2FKEGe%2BIvOVWdZe0YPesLsZpFay6CPF%2FlrXpL87uIwggpj5cAjvbwmIRzURZF4HMZCg2w7cwxpKOJ7guODeegK0Icslk9cPAFpa9s8CBwTFqUMDp3%2FYUCa4VaFqZHtyg7Slb7g%2BosR%2BA9fGsDXJmYtNThdPJPs4Hsu%2BidMuYAtvOFwxshCCWZWJ6yYQShJfRN7RkkdNBTISb5mNk%2F2gYoaDuEUoQx46bOdvyrbdqGcqBl65bd%2B2iisoUoztRvluqLE6FtWLovscN3I%2BQ%2F1JdTUXHZ6iS84P0v6RUN1edAqKhTdQhEOrgY1EKo78IuEb%2BJAa%2BqVXmduQXkbBCbaQN479cxyCIftIrUbRh5OoV3RyqTOboC7v4W5sQ1qdRRTEvcJ13vpMMfuv8gGOqUBvlXytjhIn%2FgK0OGxNtF2WBv2BhC02hZjwdzDmivIIGBaccHhZZQjphHJmUWwgFmwbcKEeQbnyKx8N%2Ba%2FKkAjzuEnCB6K1kh92haGqps5NqCZgG3O8F6X3k%2FRa0w%2FwhvQxsCEWsX6ZWjRNmnrH%2BMdvcklXJTWlcHQIEYO7m7vhgIvhO%2BZiO6P%2B3s6QSBTs%2BA5XQl83ekYmdMvuS1rxzWv3L2LDmMi&X-Amz-Signature=43e12fe4fcacf3a9d3d77a72cc3e46e4c89a7b454c915dce9a8ea9efcb32e66f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

