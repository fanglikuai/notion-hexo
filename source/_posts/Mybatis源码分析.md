---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XSI65AOO%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T160051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC3zoBsk%2B1dOjnog%2Fa%2FH7Hdc6%2BJf%2FAEhtXM7xrKsIf%2FjAIgM%2Blr63mI387oggPq8Kj%2FFPz1xY2mlw0xOd7wQICly7cq%2FwMIcRAAGgw2Mzc0MjMxODM4MDUiDGeWAsAX2VGfV7v1AyrcA3IlOpIODm7WeuxiXWTM4P8RviQOu57nHHWZb5IlGs0L1ESluYeaOgAuWmW9Pg0zDG8CJrCaM%2B1bxhRiuLzLgz3n7ocizJbqPMAYUBwvuAGS7%2B2HVC3fsO%2FS8QAmK1RIUp279fGbxXVD3rNFsIEWksf1Acx6Kivg2qS6txXOepdGaCvV87yyRiIDDsrjGeir8KOejF9i2uvFkXgsgFgJtGjlFSBwQAqlpbjqvz7aEuQ8yfhHfcNDvcg8Zd8ykctLri1vNrs6gFfWKE0Fl%2BnU9rfR0%2B3s8%2FZd8dhhWyoCjPdEteToZGLA276PrguUCUYeeTSZAJSrpssaJt2arQDXgas1jlJsPb63sb7c9UnAiCHanEXA64k5wgPeDPtqMWP2wemnfAbQSDU9crOe7uiRuPRCM4cPOeAhJ0uJpDa6kievwxJWiXhsJ0L1qxEpk7ANlkN0xnK4%2Fse05bruDwsYs0dx2Z2BslUaYEaXDDijhM4DBDnpOQGorU%2FnmzT4Q%2FYDBrRZjZtv4MAR5z0efQAqyJzgCe6ocTYSViay7SpTJmU2IkfYprg9KcmVtEaSNcctxWTTc8fpXPwVBGY2exkaZ9W0rHzKeiD8SqpECLfEXw%2FfhXX2%2FFYf48f0wCmNMNSel8kGOqUB%2F7YybjekT%2FEXtVAPqoEol3U1SGFzHI4oYJFX00e%2FSCAwHWmf7atyh%2FBwnXDS8vO9H5A6sp%2Bek4fFGoJz9xfASLrtgIgae1gULUA4%2BPLRS6p2KiYwc%2FdXscDfF6iFW5N9nYXUwVMNnwH0i0hMrV9AroZXfzLBgg3bUZj29jm%2FZDE73EF%2BfhN7uAGiJOFNmEIovo2Ph6g9ygOwtxERYtHc%2F7wxng5j&X-Amz-Signature=f0a528f57ae2f120c10f9a25857e95cbe32f1d1726c14f13c17125e816e20fa4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

