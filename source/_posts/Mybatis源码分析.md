---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663TEP5YOQ%2F20251003%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251003T050044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDst0M%2FwsLZxb6nEr5UEHzJYL4hvdQ1d%2BJQzUHtyLbffAIhAIehHpFPM5mHUiwsvhl7qCLbSOnru4L2LJUUoTHbwk4rKv8DCD4QABoMNjM3NDIzMTgzODA1IgyTpG%2BeYXrXryms7qIq3ANJbNg2IyhcNXu6rFkDsfPaadJ1%2FY6ZpcMaV3kwNbgPARtWOiNDjEKqBUxdpTQciCWbdL%2BTxlsILafnYaCBdEIjSumQ%2BuCOQVRW1HVNqiA7%2BF8u3gl64EW9ukbm8oI8o5Vt3JlYt%2Bg%2F7Xb7q%2BMdpvzeLmF71ritO%2FmWAl3BXM0mMNFi9QFzMRtZsyBWZeMZrc2lVzj9LaAfEOHlsvFT5yj4Zl9N6h3ORavcIy%2FDnYfXWlkPGTvhC9OJIz0zlL1h%2BNiJXdJt%2BNMcIzMIo0rPS9WC6dhAOzFpCGtJld9Qqprvv3tlXUGOAVt32txguEkuYnxmlblwgyE85HQsCxtyv2Y5ThlXXvSmVyXgFwjjmFayCnGHM68rvOQ6M7EaNTUziGd9Z942A4zFCQ%2BxYx9pmRV3JdG38GLqBnznm88lRiXHVj8ueIGA0Hr%2BVTq%2FJIlHabw99gbuR88tbTuvVOIhpkCOe1u5Or5dS3byyKhPA6SBomW4TKIyXaAp9%2BwvSJVK4pu%2B1AAv3%2BleCHa8k%2Bzu4jmBmuPGhP358SaERsOM%2FDJrvBOTxHkFuFQXZsmNKPnZNctu98SICnYlu%2FcvTG%2BfFa0%2FSWcSvE%2FcxpZLCIcWvd6UES9I1%2BjZuRktzzvyMjCXrP3GBjqkAX31CzigmhpWZC7xiJdTviyZwT3qlWz27FFCT9ROA0pvI7VQYJ%2Bip38DcSE3VVtcNTKO%2BXqlkTj15VockyOJrqibKDYa%2FVgHqeReJHvqvXjLyG4p59pqpigbhVWGkC496SmdWlJ%2BBYj4W9DiIqj%2FqHQsUYE1vOuo4YbSH1SS4EmPNlYXppTz3eka9zetmVuWlICs3KpIVPatWbPAEkEyvRQOq75m&X-Amz-Signature=8468890a3f19a14369fc2e678fc493dccc4cfb72b32fd91322d57b9f3cbc4d78&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

