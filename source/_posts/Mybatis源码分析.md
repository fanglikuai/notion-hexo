---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UNWZR2IV%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T100649Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIAlyV8WVAAgTreZciQnoRzZPBT%2Fjz5fsqd4ufmt2Yjn7AiEAs1LavMa8IowoxrXNbk914z5roVR3roHO4uA55kHfa7kqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDETky22Q6Ro45I3%2B0SrcA57AR9Jw9C2rKjeq52nMlchSSkWezIRERT8xnSV9moTwcuPgQ2oOUXTJdwjCb%2FOV0Mwc5wp0z21aHpRREGhpTPBMWcjbbhfpVDm3Mglel0ZIAgP5Cl57mmywVjz0axqkf35cvi%2BjgUxE873uULd9ebzgLtRleaW2p2cJ%2FoP5WCvCPTBz0tbzZ1GrPkEBFdNA0CJc01yQErev2s44dbid0v%2B4%2B2EZ4HK%2F5LcA4RYFfFJml3k9xExXC%2FOo%2FVCmSt4pkUwCRmrA%2Fr07j6OOnnd8QWiVX7O0qVHM4LjhRyKC7GHY57rxnyYgR8Ke2mOTHkGmeGqX6eBx39R5Anzyl7aMMql48mQKO2QKkUuoLgQi50o0yTK9FQfKa0ssh7OVed5s5L%2F03Tim6sFV9FWvQXEJY34UgcoLLvBitNys0NeJTYIjLxO3eMjAzQzUeB3dJRcn72YF4%2Bhi5X3eq3OvC5tuw%2Ba6%2B2rXroCtQ09fCF2WL8QlQS8cHsfJQHm9OkjDBhVU8Bo4eUAJvO%2BNWaBoKXR5w7d%2Be0wS3JpzBByR5dhEd%2F7bKNh2XiuTczBrOHzTlCjgWno8UPZZBf9aS97CN%2Fkbtcge52vxXRt%2FZS5vL2KWIfwCvNQB14%2B%2FF6zct7ciMPa218cGOqUBBUNT3%2Bo57yrEN%2Fzi7TqKPxPnMA8UttSzzYy6ao3i%2BAYAA2Hqpd0sruQc3a8iZoZGjM31WkhGbi8cfa82d3WtPTlACTv6wWiW3vP4lSM9KEeIxL6ZjtlGolxBLzji70jeMylnL7bniE6QNlibj8949bbyuyX%2FbQC50IuB7Z8xvydeuhFyEGIIWimszKPdL26GDR53PERdsFnoluKct8Z5YuCxjdH0&X-Amz-Signature=685384c75cf75f8ceee9cde90aba0efce2741e74d434d91183f63c787e1fcbce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

