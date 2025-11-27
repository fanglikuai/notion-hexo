---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VXBMKJCB%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDNI8mN4ZIpa44eyl6kWRdfDRTqzBo%2BNZ4BJmoXXiAreQIhANMs4lkbzFcPela10p9WmE7oi%2FDDQq%2B4gNfOxuAg8jhZKogECJL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz07uZ0A7orINFx5NIq3AMUT%2Bf0dLRYASCnw4ebzzb%2B%2FTw9TnP9h6SJu5Evj4XhL2Xmnu0jyI0U61xutQuXnlIJnsjRE6JZDGu%2Ber4%2Bi0YIbT5eyyFD5cl9gik71qjGZLpOTuFeASjFdMFcWGYjSOkE7MvRZQkXWI1W1DJl2HtjJg1kr2U5UduHqh8ejzZOjbIr9wJSLBogRhmcck2TZh6Ps67qQzE6WI4%2Bo5Ppwdfupj2Migc%2F7ZtnB3XG84L3dOUZQ730BZqEyzbdfw7VRIsAnlMiR2E2j3cCwMq9UNRG3pxtHePlVIKRrQJB62bkoeVGmS8cuc26UncJdRjbS5fT7aUYxaB7F3j3bEs%2BFDSxE0WDBQADUMDJBF%2BO%2Bj5dNTdEiWjhNI9Fob6mVjoVnhZGjemX2XUJLmC%2Bwun%2FZI5ldsUr1Pz%2BBkCoNHSojePeA8p7A5oYA%2BxIMfZnPa5YL2%2FAwVFFIyEhXpvw9S88V1ZF%2FdPUGSWW0HopHkbApkvirNvA2bIlYwzIPvjdO4L016Eca1cIEX3UEpBfCbIumg20H0D%2F%2FVzaSpa9ifK5EL49OfZcQAyoDOQDvczLBXUuX2vizc3RXWmD%2BW198Y7uhF9BFWHnqt6G9QdsGsmFho5a5IL9Q0%2B%2BYdWo8S3kcTDSup7JBjqkAVEN9PKLpQGueeOwBNsvqD1uecTJNQheq8seTfXohUwN5D1NCEztC3fo9FGuK2rVg2efoZjLdAkGU%2FhpjKEUsLrTKnV5%2B0pudh0zgD5%2FLmymgu9CXDgG5yimvgXU0RfvC8mm7n1H39H%2Bk9k7qezg84%2F5%2B%2BcnxEOyoGfUwr83RuaJjjeOAKYXfkJx8BIYskI86YmHVIOjRCBKtrxyFyUKOuTzGc9y&X-Amz-Signature=62a6e21c1dc17320ffce516833eb110efca8ee0823371ccb52847ba7614a79f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

