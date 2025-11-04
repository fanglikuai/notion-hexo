---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466THIVCJ4D%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T130045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCtclKrU8SvaMU9M00jjzFOXRpFudG1F8OgrAs04iy83QIhAMOFLCYPh2Z%2FmAn%2BY6lssMPQcTNgs7SpyrXPRIUpfK89Kv8DCHUQABoMNjM3NDIzMTgzODA1IgzrUzLzUAK6at6yK5Qq3AOPVCSiXmhEBCSzA4v8m%2BFrSYXJpgtgSc%2BDKkXP6BMD1bkw6h4XSQhdys%2Bzx1Z8JEDbVcMoWLtaWPtEgg3NDU4M1ajxtROocCLnf6%2Fl8fA7CayDheXE1cqy3FWI%2B6gHOESZMWrniV2JRRpRjVCLOf%2B%2B1X5OftHrYP%2FECHd9O7ZYbNJMq1FFt2EWzGWIHM2nXtc9eBXdAH9gqY3ySGZJZrm5nKZk4OtjLGilU18dAJlTyZLUC2FBdhv6Q7IQ1bYL7c4LuW5pdSoEFyDbPIcEAGc3dZiEM5%2BQR7IuNVeSrPtkkHihbMdTtN%2B3hUUAH74q%2FQyj%2FrEzAB2mn4krPu5nxRS4JylSb64Xb783q%2BLTBE0DIFKnGBeadNeZ8zuUAeyEqLswXmCEDEksagVSIRLqv5T032jUSgYqbrgK6I53mxgsueEGtcNf8hibcJc1weKnMCUqZASooekjgCjWjKRVbxKE1CZSeJ4xHEs%2B%2BDTgZ7DiRqVLupaJHLJhFHVP2YMe1uz0R3ZemOrNR6%2BqoG%2FYHHufqvBoWkK8%2FKNqHa1DrAm5UHg9Ve0qp0vlDFBLvEqWobDPAeUd3T0X6elSQBiOLpEPREFJRU%2Fp9gAz4bE6JIEM06qiA9nNxUJURwQCGDCT0afIBjqkAaUGP%2FkdKT9TwDvo8ZG2hCiZRgfM1tVlep1u4093WPOFi6RlOvqBdYtXA4k1Gbmio4Z53JymAV0JJPJRM3VsSxIdH2FuMS58yarMpqK%2FIRosDNwd%2BiCC%2BgbOnDKocqCYG%2FXSnqek%2BqXd3aOrEv1uZZvYwLhHK5MsWj8G%2Blx7Ad%2BLxKrJFoI0ggEnV7SmiEJFJ7AajexMX3PBIB0xtZj0o8vS0NGb&X-Amz-Signature=88cb7150c08c46df5d40cabde4978d1625957c9e47729f003c499fde37db6ab3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

