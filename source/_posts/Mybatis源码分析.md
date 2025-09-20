---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QBNXEJ5X%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJHMEUCIBMvnOBBV%2F2%2BvuP1H4QmJgx5hnj5x2y%2B%2FLY8D2tRSbYGAiEA0WJl6pWkWVByZUXuozzMv1RZPDyjI56uK9anOyBPfD0qiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDH8Td6dRtR98x7sZESrcA5GpDgHwlsVwWX8hpbvm6jdYI%2Fj%2Bh81zptzXWAhIksKqjabZ5oWbnkAuHFxlo3tc%2FFVErzRIdZ9MNeaGFQkis8Tf%2BI498JVYdmGsWmLG0gAoqILHdtf82VwkO30j0CRAEfmrub%2FAdqMbLLu0uvSFYl8A51wuzQmpha15GL9wChYaSL%2FbLCjR4AonexkwIPvVszmmMhvkwQObC2kBhQGp2uchoyfQ0Ph3dd1QJc52F65DX9NEKWUstxlWX56TdX2ph1nwtbuZdNTBlV8sM3dJBf7I8TiXMStkgGy18U0wvw%2FvAZqXuiaEeOyuqJMq%2F7Bqyw0fAuJKiiuNNFl4ym9WFWybamuxu%2BD57efkMp%2F0blN5bxqG8K3C6FD5UgK5XxoGZxkEkdYtj%2BANDaFQC5415SenWjOgiK8ASLUuIRQj3JSl7Zb3%2FcSomk4JuG9N2mCq8r5dqiN9Dqlfcl%2BSvHZzljq%2BtdfJ0%2BIzlJxrhACwoZIrK4R3jOgRDqsI3RZ3TNBHzEB%2F%2BHgBupCuDP7B1v1770LLUMCnwAXyprMge7lbr8aKaQQtnfFrYyViYqHk6XnB6XKahMS9bTAx10PkYKe20fggjqjJhiP66gdBR00alDPqcDyZ3ex8U9VHB0ZSMJ%2FMusYGOqUBFuRy%2Fn%2FBvsnxxi2xPFPuVe3jlSP4APXQ66v8L6wyjHRQ89Osz3T5VU7iv6iZGcE7xbXijoA3SIFzv5y8and8p376bHzhG9ML84%2B27wncG4n2cugVcLRk62A4EpoadFT63UjEmvO41Xh6IyaczbLdymCln3fq8zlC7%2Fb2eVijEbs131OHd4OQm3%2BuD2Heqqm3RyveTMl%2BVCumIQ08EomxxeYAMFSE&X-Amz-Signature=512c0f2a944179aeb4821d3f1fc27c468dc9b0980526d6ae93e5f5ddc1c1caec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

