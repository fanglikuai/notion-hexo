---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TF3EANFK%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDxFPzEOCKccph1Q7kwdPXiewWnY7O0PV69J45RO6fJVQIhAO%2BEu3VJCYh70hWolBsKBdYOQB5vcfIf15diOjOv%2FAZjKv8DCCsQABoMNjM3NDIzMTgzODA1IgxxRfPyzyudpG0Elvoq3APUDN%2BpsyKw1uNlAVYzJ2RNH%2BuAUeq2IcU7jjQ3mClvNoKcHl43VolR2PKC2WsTgfgqEpbSE5oTPz7Bbxf5%2Bl76uwYFRKd8Q%2BsIe%2BOGRelDSe6ZtX0SlzQl17pvMKzbyttJIpVeit8nLUMagqUviR5%2BSQGt44%2F5Xdj%2BnL%2FoEWEnqApijEqFkcz7dOzz5coONPU4yO%2BEhDwGPewf6DSp1WwCnJNgrVcGT5KQxeTAM8bKliLu9NZnkj1F0PliiwncEl62eYiglvoSjWvk7cDFWfPjnPU2isVZroLQShaOGOH7mTmhaqaJTCcW18RNDXi%2FzDbokulLSdvp7f76K9ERup7QgmUFKEcOD9WCuna6JgfPyZNBlDpyn1CeH%2BJRU1eSx%2BINinLedfTDVHC%2FhwmTtT5r3L4qjO%2F9ofXjvuxlBdGrOsEEekn%2FP%2BE6WzLRxSoVDKdWJrk4IRgCNqRtK%2Bf0u3EZuNjLIJtYMZxQitnk3bM4gp8DqwtuNGDrBd8dZmcXne7TnKj8XBtIz1sCXgAyTDTRlV7cRMsOsOnx6qOIxk6IfRsUzOAa7xI83laa7lCVPDZnf8p31wd7bWw8YBGT4T36TDq5KWxyOnaeHFrm%2F%2Ff4rRwG7cU4NNxxpsda0DCv7q3HBjqkATDzIDSWmdz8D9rYxzyCH4Qm3AhcKeVHpYSnQqrZWH2a40%2BaJ6N22OYtHwyMD2GvZ9n6CWQX33GXP9aCrfb9lvV32skdYuVwqf17CwkY6EcXl5ApNAyhC1cx5iVD%2BI5Zlz0lM%2BmFcITH5qEfO%2BHAHmocCu3WU22DN6iLMicGwYoYGHhGeXGQVkYfPfrTTWktVQY3DM4SRsAm3vfvxPkNOGYt0e2b&X-Amz-Signature=696c52f90f17501e4c187df7262b272ef7c95e610cbd3e6afc825e12438302a5&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

