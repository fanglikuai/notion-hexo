---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QR22CFBI%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T150140Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF8aCXVzLXdlc3QtMiJIMEYCIQDvcs2APVE47m%2FJpzivmLFwT264MKVOndWEw3R4BwpacQIhAM4BsB3J571dUp2pMmci8Rhyy9fJhaHFxcwK45aa46bwKv8DCBcQABoMNjM3NDIzMTgzODA1IgzStj0ZgeoGqbuQeC4q3AO3gJNLFmjqtkduI%2Fi7SQEnYa%2Faeid91m8AG2KVsKjCdghXif7y%2BhstAmdepNPM9FIqQaZ4DmCfCeoG1abvWBuAznxWr1jsmXhVyKuXs4bz8GZCta5cCiQL1dsab0Iy4Y6Uft%2BrVQwWpqW3Adpb7jVucIng5kye0IF%2Fj33%2FwNz8xvTIEgERb59W8L9fRjLqvOA%2BNFtFcdL7rXYZwyWoe6b8ha89qX0XkSIXDlwxw4E9SH9tDGddK%2BNttLQwHKcxPeZ68R6%2BRWbBTmvu49X3ViN469E7%2Bnd9R9NCxpdU5LYlY6Qm4Eqd9Qc42OCGeuvKrt5pKDGTCaQ9DhZIYLdK0YkKSyl%2F5DjmIrO9aDohMBVA0l3WNJTYpQLzOY4PT%2BkjhoUOCJ7jzsq6WAOFIiBgKAqD3MznIYtmBBdXfXMpr3h%2BjikKtxLXPCqMQPH0DFWnYJDsCd%2BeCxjPXwGbst1Lcjrjf3psMnwkZokqaQ9xr%2BNALSClsqj0jUbeABOJvdQt9Gdoz4HRmRE%2BPMCmrh3y%2FU2rmAErG6deXUMKSW4OUNrwbh9Xyir7UAQKCdzx5mGMGdJvB6DtE6sfSk14IC5uf5efYzm%2B7i%2F2Uz6MpefBr0wOMEsBDYOy%2FHShMZkzhTCprt7HBjqkASL8KRW1l9%2FdLPj8C8MgEokV2JVGwJ%2FHDPsoXqK3PMJAfQUhgwfbKz1tnQEoRBh1HI9hY0HhzFLLj90ZiWHNswvAGx0z4Pm8Ez3rC5UZrtKvZ1GB6HUHIN5EFS3Bdd13q%2Bqp9JNNRXcbk2dPuF7roP3E3XjETLeT1GaIl8%2FPKiUhYbjCGnZ5EHI%2BR%2Fp%2Bq0AonGJNUKb6UyFmmp6jKLC%2BDJBRJG2P&X-Amz-Signature=1f139337c9a7f32ce8a51b44b4d70b0eef0e45f1f17c6b09d6a15f04cc706830&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

