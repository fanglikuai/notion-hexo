---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WZXRLQ5P%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJFMEMCIFHbcMa%2BU1%2FDtW%2F7g3K3Cxhp4slCcm4yk0hYa%2F6FPrL5Ah98yqnj8QgGLeQ4WOAsvhfBKm6xVZLOEZhMBJAX8AhSKv8DCEYQABoMNjM3NDIzMTgzODA1IgzF17JzSU1neMTfad4q3APYwqhL6zYzgLqNHKJcH34kPzQI72ZGu4K1RQz8x%2FpZX%2FJc7ou1PVkoueoRtGn9FBidC2fy2D6zVECgJ2IX3qriKTX6g5dJL%2FBnf6g7tvrMQuEiIGD%2F0m%2FwG4UHYqhkMkA%2FpWhzV5XUgl74ygx4oUfKOC%2FsVmAirOupYt81L%2FRAvQxCh0r9ohtUuEH7MzNBJgD%2BvyILuiwY4hATV4OoJUCVua1yG9P2jeWxt4iJwMnBzOCEPn3XaTCRTeuZz0ZdSo%2F6838JfVknp%2BunP68aLvZm2T0HgvKJJI8HQiMVgnV6ipypMhYfBDRTS64peS9oNksmuSY0KjDoEKJaHFGF9wHjGWfb%2FVEaIULHV%2BgeQQApHKzrIwshqu5Nr7fcc1cbNk9Nqa5E5UgKOXah%2F8p7rDssGQ1sMhGLEjkoyUk%2FDZvV5aODTIHGhfoRkYt9cYfWoiiC5zY%2BEWGkgv5SotDDMmLgPyXL2OqRBwUl4Nhr10aFS4oY7vInbIMfVV5j8IgtAGYiSpeztO7hearzp3CPPosbMp0T2cvRn2o%2FTf4CiB4tXODQdvo8co222TUsyjqp9ENVtiAeFbgTQdoB6H9SkvtL6W6377SgS2%2FdHUNJXIEdLj4xkpudbZH7b2AmXDCK2o3JBjqnAfgIOPiWLFXhJXeNU3VUOpKP64FHIJ3efdjqR3Fia3Peg2cyv8P7vKI3anHEoYTicg3zI5YJwi%2FlHvtz2pJcfIlm2tQgzaEwk903mXNJ0j9bdgAVYW7CfFsSf4u4SRbda33oQ1E6YiJOVjCn1bqRLh4zoTqrY8ZJC6M3FmJRtgw%2FPlsUn523x3gn1srpruOKtpt8Iq0Yck5ZfoeUe7yQ1AVp93hIa1eI&X-Amz-Signature=4a5dba16ed6159512010b5d1b71656e15385e4fc47bdc04b5b363c4e20653f98&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

