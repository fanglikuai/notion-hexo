---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7SDWVPG%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBDWhTGP6Ta9BzPHM6os%2B68hIlicrUzdejbspVEklYFQIhAPFDPORkuv4X4tmb8Os3uaJREtPAzG270JJlyx18bt2vKv8DCGQQABoMNjM3NDIzMTgzODA1Igz3mPa2%2F63lflnBFgMq3ANcK7peP0YphybJZg66uV3fqI6FpuKoaZ9ZQo6dxuyS9oYa4mSe%2FgZz04bwU%2BOECztQUbjCf6OWroyBZBxO30EUYSQFoyM22mucD4sVxmzH7SRzsngI3kZZ0PbTOnfMbTYctuJa6vazP6ccnDvUVeElj%2F74XLTjiZ1QIiuWo7pXj1mEaOokY8b2yss0iSpwAST7QO%2B37Wp%2F%2BN6%2FEYUxPbbgjK8BENnEU3utr2wjGFDpE5kec50VtFPvC82PbAYh8b3XAm9Na2oV%2F0bw9tGq49uxI%2F8yrk15f%2B%2B7bgmlExwINIbUgnzgxJU7zYlTGAiObhLtFBQlfI8n2RDGAAIAmKbDM1Qfhz03kAqMSLouY0aUwBLeXQd8ZqmmF5cRp72zQFPVpUqMwraEfQqbhr97sexqZocVFS%2Fu3d6YLkY%2Fkq0zZLAiVeG6bKHeeekuxTylu4L8ovSIp%2BWosbFemi36g4hHEPUJUp8kT7crsvvPpha1NuDwErRh7IXjTwyfpH9SPU1KWLVRHEtsYs5XCF9jITZBpJHHBwkm4wjswGqbhDKuIUihpUEgnlAa7LQQApzhuaCsWdymlebOizIm7lqkyVNp3o%2BpEYEW5rGI%2Bw%2B5kKYPpCjaLgDildmuC4veVzCEke%2FHBjqkAUOLhLxeRn7t2GSbMvPd7%2FlZYyTTzsLdu73CUoLSDAUvnlvuIhXXp9nR4vFijow3YUJ%2F8iVHONopln44kqF4J3a1FYkfmZBgRR375%2FK2hOaYxy3uYbqcZsAMlrf%2FsrSppRO4ewY2KNhHep6OmqYhduNkU12tOeNto95bRL0ANM7Ppq%2FL0s8fTX4yfudA0KpdAdnyqOwfCpHw3GpdmSX1B6mRpT%2Bi&X-Amz-Signature=9dc24093ec93427da461eda8740dd4c1ea7ac1596ccb6810b58b748992ccfa1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

