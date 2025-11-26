---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666SSUHLRY%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD2AMWoPR%2Fx3r8T5ZDY8qTPnq0I7yZxtID2vpbOwCwWbQIhAIctPpLfVh8m02FtbZeKmvYYXXl35DoU19q0a%2FVTuRi%2BKogECI3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz9O5ZqZ9BFw9yVgNMq3AP6muGcLFOQwJ3sP47Xke2xzoA9hCvMITa35hcPzYzjftUYQCoQ1fKoWDmHnOBYaDtAWZeg25p%2Fzz%2B5Tew1c%2F92IW3AsFQlJTY3sZdGGSLhpASIvP6jLjhZJ5od1gwfCBBGLe94mVUIRs%2F18Z5bl4Nhc5YDATOC%2F7QQ5Pbx5TmcWPbk%2Blo5KhEyOi1gL9sLE%2BlwjswPdIozjUmhMQB3VUXBwpbDMa%2B23KP558MYKIYnp20o%2FXUUj3uICqeQdIVPVszfFYdxHz4CNaX%2BAkVABXNjTygvzieTM7QfDhTV2Ngza7%2BCw14Pgiib06Vj2tIgWe5OOQaKaRM3Km32JizPuo2phBDz1DO0emM7VWVMYui6CyxOSwznhhW7nqGPefgetnWW8a3ISmlcr%2BqeO5p0NqiAAJO8ao4SAvVXPC1JCg6%2BmDb2nGv4o0N59QeO2sL7KiqB37PswE08mmmKkfzgbl%2BPN7VQKQtjALD%2BK1%2F83YEf7YC26u3pBQRmtJhKOhEs7GLc4BHWuj6UKFizZB72blSf3JlomiQkWSRSc670%2BsQnDPcFxLlSpLC7R5Kk6kb2A5ZpawKbPemTPALoV4qv50DuQaeqsuffDQ%2Bive%2BaixTxE8FqR2SwcPtX3MAnOTD%2BqJ3JBjqkActYvwaWKcwCVYzl1AJFBs8S1gmCOjCyHyCB5BjAKR6EIH8n4gUu1GjZHmblhRoLdFW98IQUDm1x2PgmKqfMIY1OdmIuGaF6d4zNklBvgVPhpB3cr4wVkSg56hB55OABWwiTipcCsqf06jP8wdYDuGB7jcIgXAurzzs4GxWfGoMcQ4UomxZYRpM7xAgph2C9VotcdyuzedXKpVk%2BpZp5D5iuIZzs&X-Amz-Signature=572de74facf59f9d5859967f264275712307ed2df61799f339f09f9738eb8ff4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

