---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665VI2HNJB%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T030042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHoaCXVzLXdlc3QtMiJIMEYCIQC4TI0uKMo1JwoGI%2F738x1kw52BihDnR8jGWyotzAXmwAIhAMgn8WuniM7AlBBI5zOLa05ailwtwpzd72QKqSlZpVK4Kv8DCEMQABoMNjM3NDIzMTgzODA1Igyu8aIEm1XrKLUCu64q3AM9KdOrtp5AzfXMsh3XdYPKnB6fuR5HFp93QNoWkBJe3ffdln3tb4HV0IYpz5e8l%2FZD5mIzJG4kWp9ADYoXqp6XM5hjS8LRP3zBZOUL9V6C5DV%2FvvxnShLzitgT%2B3WzQgJBJ%2BP67oXuWn%2BOD9Z%2Bwl2fX9iispTpB7Z%2FygI0l%2FhnwxNzhW6VXlGrxRFpkYyFSWoPKm20EHJTj1EKpyIyCw8dNR5CpScDTU3tskiBmBkHIItIgloxoSOTF3vrsqXQh1NYOw82HqCT9TQvqq5UaXAeA4D12%2F7s33fWA%2BtOc0vK9F9fkHOxJUvXS%2FGPwlk9PoyPe8cr5egF1Yf8URfXpOD%2FsHAz2L6UHt7UXHiPo%2FWpdCBVcGtGgNYsmGDjMvJk4J%2BArAiyHK4r3hqsjS3BPA8zl3%2FZXwmFL0iewFdfAZnDH%2BbQtAI365kzmcm3dVuXO6R12a%2FNEmiRC%2BvbjsuvAFjrV1mrHVQqnZvWR%2BpqRlCz94vNwof%2BndBsNQNDK7sz9ku8tLKzLoJMXKMSCebnxa0goTCMqw77hVYxnnEKgJTd8%2Fg%2BkjkJg10Z2gghEnajYeFPmGxOae2kp6bxsGraI1P1EnaSIUxdv2RUgZPkinvnLwepPuUcpesMAR422zCJ%2FNTIBjqkAXYPy1cpvN964iznqCJiJtOjzNs4l6d5UhJjuVtNm4Pj2ki3PmcWxrERSIY9JEombeSp1Yv3bp9CSx6Vm79SkGtBK0IhLMxRPNZCJA4WUtdGYZkH6y1FdtvuX3GwlSF9k3kNPjWnz7OkpmCOTUt%2BEsX7teMbbz40Ve%2BX72475DivB92UeZ%2B2dKHPDp0g5KFykAWgM49ALkMyRqAbynyjDf1loLnz&X-Amz-Signature=c63866a019506dcc6a54ae1b59b2202fc9a76045bb8d8faac928ae5afdd2a1cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

