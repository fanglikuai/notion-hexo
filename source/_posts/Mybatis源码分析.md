---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZRNRQ43U%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T180047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQCr7ts4Koj3adkfutEQRiA4kT2nPoiu%2BXIqm4d44idW2wIgap4LMFoLC2nNkAqdZbyZl1tkiiv17Jr1SjOH835hhT0q%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDGJWwnXziZw6oT3CVCrcA%2FZJk0MIMQa%2B85ocaxi3nSc2JtWlcOLt3NdjpMUGYg8qDtpq5AENxgYAj1JTidT0VOeomwI2fbVleeE%2BjJZR2g6TI6tVFmiOAcj%2FXV4IS6xkVZy6dScUuWQpx9PHSccQhJNZUes0AaHRXdMQ659xmQ%2BwEnFXOwCwNkxXXeuLCVY0%2BfBkpr9bTiLLMPEh%2F2GEJucWOkRarZyRF5Cz4reXiqqlbweQWS3DmKIFdfffiSaQamTvh%2FQpKwZB8WFNz09QVRSnpiOCwWKGwG99%2BCfHZNaLOm8hK%2FkPeo0LlepQTcBOyChjh7TcN4KAN%2BBSJzMHzp0YIdoGdRunfPN3yqtzEf%2B%2FmYdMczyWEm63fAjpYbFbhEavl3ccDAZIlBSbTebWu4oCJ27mJ%2BPZBqkHV1aZf83J6bxrMLcLNt21A96EKcqIKrl3UK9Uwz1IAtIkErzyDi2OQBQU59AE9mtAW5nAAYoHGxjYKsUPVJq9XTHcFjKdz0HDIvAkPmOYG%2FZjBuGF%2FcDaPAlvQHtPkiw9kM3f7qPXBr6x54RZYJXtc2AfTvmOQRwdTOAj3XP7Z%2B5M6WBfzJ2ekc53ikSQSj0kEUX8btX1ACCHf4dsXXxmFWcOiMElcQPO5NhBN%2Fz7Q4WjML3LyMgGOqUBGHk%2FJzCU5pQ7zeUaCZhc3ULykK%2BFvWXWfHInD7khanoN6pP%2FaNjvOaCaNZlVqfkZo6FO%2BQ6jTcz9redHobuLYq4nM4ygHtansY7f%2Fk44MjnDhEXXOSoZbWe2ZLO6Kni64S9wv0aBuJ293V0ML%2Ff%2Fwjjvn6FEmSdJErPBRIicGr1Xpek3lytSTNf1GCTxw6b9npweDwcZF7fKW6HWoTABJI5caFS6&X-Amz-Signature=99ad2b1778f9d65c7cf36c347b499ba99b6bcb1675566618fd0c41957c406255&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

