---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466V74R6XU2%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T210047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCvn6GiX4QA4QfCZTSSZc6Dq7Ih8WtsCcpzbYtDZd3ADwIhAMSgSeqXL2CPQ1538OWszclZ28AL43uucko%2FWXtEAlCVKv8DCGYQABoMNjM3NDIzMTgzODA1IgzT1wtEAS6PIDH5x7wq3AMIMHgr00Mi5COxhvYyfYAr7qZvbEg6Ki2KdXsiIFe56eL07WZ7ZKEzOnYSn9hJN1gM%2BBdertIXuFso4RZ7ziWPo20NI1bnJp0et7J8cGLdiGVJOd1YtLe2D0xuD8OZFnpZEGfroT9aanoeZAUbGMeBQlCyl3GhzZkQ1BBOCZtdCZSBT2JDptxti4Iicozy3IGavEGe4YMsItaxiXD1bDSZImBUPxOgWUuArQpDu%2BDFBbOYovd5QDytwddKOkHYGOrObBSCrqj%2BXDJC9jUsnw4BCO5%2BcCDF5BZQtGl4yUaEd9k1hTTjsm%2BSSZ4vXxs4vIW8JISqZoDaCAUM6dtIEIFHX99qu%2Ftp8SS%2FwOdCgDr7xJhTAVfO6fAnwxoLcb%2BFBK7aN0%2FW5v1WImGUbd4cbAQWdbeOrCNaga8ycZEGz0sbZpMRA0wHx8YfK%2FGVqsdTo6f629lu7fH47q3rufYJPTKT6F5qbfSwItS%2FOS5QH%2FsJFxkZMcVcTYgFoHMFnOGftOdJrKrZzdRM%2FjKsp5F4gMqq4ujkhS1ZW45HY4TlIko6IKIu2EYR%2FWY%2BI02gH0D1M9TE3tXr6wCVKAxumOQjVOXB%2FteKc7y2Mr3jpFOdPMORa5stbQWb%2B2WCN%2BK0rTDW47rHBjqkAdFJ6sdbtumu46zn4u%2BYK3pNfLzhXWb37yKHSRFL%2BH%2Fh4RDqP43VChCBdfdQwy%2BiO1kgrlherA%2FRusiuznpHWFnGsPRnO4StXy4cYlI2CgAV%2BWiB0U6QEcAX6HxC5dpFtsmfAtj0INXXHKOaOmpmAMFYFXY6kyA%2B3jeCR%2F99NAmjnZ0dkq%2FQ8eKYxnyZYuSUuFcdDgVYXoH%2By81eQLptQR6R7jjB&X-Amz-Signature=4287399ae4baeacba5578a574694e38c41885783712cdb3a139c534b177a2ab8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

