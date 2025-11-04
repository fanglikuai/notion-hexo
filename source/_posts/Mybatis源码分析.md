---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMD3VSCC%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCicdwijO12OUC0rEB4tT1Et7aJN7pv5z83OBOlW7eO6QIhAIgG1wV9s5oaCrbz4OCOU6gwyYFhBh2lWfCmIDVcf9zyKv8DCHAQABoMNjM3NDIzMTgzODA1IgyTWgHYwj1Ep5HXFJIq3AMuBK6Whv5xlS8aL3nqxePbppettX4CL9u8Req5XTvaw6YUBsiVbZ9U%2Bcc24TFywB338HKaJfpM2yR5Aykee0DbFutbsVJG3eVywZcXwQtGlEVyl1uJ8rGBwRaLOZ9Kv%2FXbaSPKpgXKLHTcAxWrFJPT9lXL5o5Rhuiv0jhlhTMGYozfnfeVFXcoDffWpsAPfGsLqzRh7LGj5fPN7I3XC4xmIZ4e%2BrtHLvflIYzL1uoNq0qUzoS8Q75SxACbIv5F79DMrusVDnK5mDtFJL%2Fi8%2FbM%2BqVUueYAclgJi25xUjrFXeKHYnZEBM8IwgUU5tBpU4EXbFdbp%2BxifEB%2BzNsnmgxwDF9VLILtDBoziKlmdbR45Po5fNfmtF3E9iuRPzqO1Zu8rXs56%2BN%2BLGscDktjaGFlqg0UOBo2MUXwa%2FC9PcZP92iWC7kNpfH7iuayXKp%2FTotRqDnHTw5ZgtwTLcnaQvXiabxjIrluSWO7UUGH5Ok9iHB1FKwF7U6T4BeAgEUIlhJFV2Nmd9Datt2g6XQKX74AioKrvbrdlELVCSPwxfrn6A%2Fnv8Zf3rpSp4Ge%2Bj7LWoCb7OrwOb6%2FpiE%2FruWVl7jhgbOShcdaBPu1C2qIpTYANMjwY%2F%2FEjxXkedRIRzDMy6bIBjqkAXdwejH5m%2FHkRtsjil3kuK3mmgIcPRnqMlGUvxlnDgrCvMmM8OqesihVUZqGBrmyBiyR1FKE%2FuXR%2FEAfvncifKC%2FBTnwrYJIAI5Y1O1NISDW%2BKqu7kEpz583yPpTgNbNJW3C5g9fPYuu4HoSnM%2BA%2FKf2d5SwCKypiirAr7AKzI5xbiDN4O5Vm%2FegvKGztH%2FH6jvyt7a2gWtywfANWJQY9CADEsU8&X-Amz-Signature=9c6f733a649ee96583bd2782e71c8f02ed86e23c393857b2d86db7ce51932f84&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

