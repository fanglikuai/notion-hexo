---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YQOLKRDU%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T140043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIQDNZr4Ft9NheQtiNYAUTGRl%2BFcZ4AuG9h%2BWmyHTnjAbigIgfF5MmtdkubUsggHSi9J5XZZ5hk9idipuWC8jLbu6ViIqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJT9odh0Z4t41NR4uSrcA9ja%2BnnNuKvZRGejlOgr6J1GTjHnE6%2BJyw%2F4xYWZQbB%2FQYDADVZ9wbS6sUwGULKjYWVFipz9gmcKKnQVUM1iHYiWqqLi1rUZd02FwYuK%2FSJc5A8ZJy7EUwdzf%2FNsWAx80c%2FtbIwq2zokifrsRM3rHrLHaMF2SVfozR8KgMn8ykxUoX%2FC%2By0s9gNEuiXzW0we3Dp78uFBjOiILDfeMDelklVwI5L4EoZkHsSsxztcze2mcs4oBHx4AcDjfOxSqgJxS1SEN%2B8il0ayPkKxo69mbdTNbQ8mGdcp7%2FkA0rg%2FwOS5R00cKRQcXj2IBNi9MiYDjfGxgJ%2B3pBVMbA09QlvQpKSq0rG10VXRFt81sQvaSUrLOHvIQzHCB9%2FbFIdrW%2FQQifv3m%2FtHqY38fyuJOiSs6jN13zppR7yJ7kKeMhvv1Pxzje87oGmyg2MH%2B%2BOfuWmxZiaf57NnAzJGDsRiRbZcgOd8z6tQiyDPJ4b%2BS%2BQYfT3wnJnAr9%2FB%2FwwHUuNGFc9i9vXrNyJ4TL%2FQ1q1li64Lolox7bBtbe9Vu7IYKyLXXh4zVzpxrm4C6vmndzM5oQhmpfHHu5lKaB3FZj97OISW4Sf%2Bv3d%2FbDg1RlTn6tpRnP6xDP1i%2FrPOsOTDZsxOMJzjtMYGOqUBMshz0itXesrwYhB5t6UcCWzGVavRLNdV57NWtpAmzMCJ0UPDcr9SGUCGcQpD%2FuohuLmf55%2Fr%2FIlF6tBhFpM%2BwTDE%2Ft47r%2FYM1RLoMOLujcJTgBjkE7SpgYtNCsAFepQ2w2EO74llqlTKGGKV%2BxnAw%2Bx07wfy5gllYqJrVgP3dbv2I2O%2BD1fP%2FSNTUBDPGkrkY%2FKmVqjRzJj3GJwwjVePlE%2FJRBwY&X-Amz-Signature=b0726dc0702865c67968e01a29722001cf140d7303398553a7e86f61d74a032a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

