---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662XDV2HVX%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T070047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICiA9k5quzX%2FOumdcLqbPnXhRABpK8oCF59kjvIpKtwEAiEAx1alc9VjtM3yfpVlYMu0umop0avYTbPJ9ILk8JagbKYqiAQI%2Fv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJRHd3Axr7gV7Vig7SrcAwnnbooe5PDPU2ZFdDMRAdv6nK4%2FbVwEaeN8yo9ZzPPIbUR2cE%2Ffv4f2r%2BT93BXIhjm2JvuM3P8j6rU6xbBOPJUyNZpA%2BObSGWzHLpjc1RQpE0mjQm04D%2Frr8r8wT8usC4riN2WdlY00eB2aF5L2Qsx0uBgBNkzcKdiKm%2B2YNEQX5tX%2BHla4RdhxXzz%2B7eZW5hXUQ3lK8qdikSMwWnzWcogZhP%2FKx0avQIN7nBNmKN9Kz7iQK2b%2BwNHGO5oUWllPBM4zzCRhwjSNJzMdieQnK6Ud%2FRp0cACUTH6pO1QUe%2Fkn6%2FFRBHjGGyi7u1ssvk%2Bh%2B%2FldkDqdEnX7q%2B3hkRi%2FGpJ6%2FdAubIG8Eq%2FLsknwptS7hGkAaiRJO4OaMEg8fquxiAaTl%2BQUsj%2FyZ6AfnHzHNVIbEfv6KpGxYy0icXb99ZgB83gv2KgWo1NANJunjJ7bN4AFLfZPPFHrV%2Bk0%2BRuSP0AqWPDwELsDSiGCFj9nw%2BB3RJ10vRhsRq0uGlQaHll7muxy%2FNa2D3vvZeYWGc3wVfkhTskN2mRToDLysJd1ZjIpfJ%2B15lXR6ni1H50sB%2FlxJQIF0D9Mi4o7MBMoOHppR3Z2U0kZQz61BueiNy6XqBKjnoFQJr8wXV6MewGpMOP%2BvcYGOqUBJzArr%2Bu7ZEPzVS0CbtdjxoBujngQ6Vjwtxj118O%2BiLRM8IKmupvYcKb3Fnb9NEnDIWFASLXZVcW3GDMNJvYx5BpQqFR9ThfsC7JtMYIozrcxUWh9SrPmHENNkoOQtjT9ZmLUNP%2B0wq36l7b89diLgxFQBHpjYPfN7T1pKudX%2F6L2U%2FDXJz10zfUvajErKSv9L3MF7du%2Bo6Ayv7EBEShu1eaumRpV&X-Amz-Signature=f33c87697fe8e8aeb9e3a2c228aaf1147a934fbe3023deccf7bf4260e8e608bb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

