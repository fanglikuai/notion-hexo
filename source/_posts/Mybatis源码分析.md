---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URDY7CS5%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T150054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFeffvxpK2QKNFZtiR%2B2mgeV6hP3zHUg8uoi%2B1bSFBnIAiA4tMange1vnqkIpzG9jrTh2t%2Ffh%2F2gU9ljii1KRWi%2FKCr%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMN6AIR2ZNDdNN%2FO1jKtwDVOOiM5h540IAoAkYN1vCC6uueU8ENAAkW%2FnEBK0KmcFbnCSNHVelxhEzCK8IdzZukf3HPQykHx6jWNa8PJj1ywD5%2FvNXHbL1fOJLjIpBnOEO6fdBohX%2BNBLOIiler0K7wd%2Fwv4aGoWFb9D7X9uqtpQr3NoHpr00GGCpo%2FNKOKupVm3%2BvGGCcz4iki018VJMrL97ZtxHR9z73cF9YF3IO5RC6fJypguGqNxgeqHRzge6BClRf5vrTmePgAJHCc1ZsrH6wQ3TBrZo8rN2NPVc92ZJDr1SLPEXkExt%2B7ucLkEC4yBgV2CG2jN5W3dMgV1%2Be6I7h2UoF%2BBJG7FXGJXNWGAxfVBf%2B%2FwzfX5nn%2B6eaHtFBjF83Va3SFjZp2G0lB7peabtnG00mMBWvMyCpsFaGDId7QMXPKOH1vK4wDuG1NQBH1Mr0GM0Xqwt5wsOwZgT6a0avHX1ZhIGzwGZyIIyk7m06zppn297prmpJNExSC994fdJF89YXwfL8wp2UldwGlFeWC%2BIls1I4zRw0yJch%2FdbLFo%2B5SEayQ5Yc3ArEJ03iaLukVHDT6ACYXyro1X8dylz%2FAcyzEOcf6mDaP7cvOvVjeEsMDyNr5xTUE1L1KKw7yeBf8Q9A20cKEscwg8DFxgY6pgGIVL%2F8LM1VBX3ccgonCIfLhRUzPO3WjEoelNDoNm6%2B3Faeov3TN04EaxfMQXG2jTZ03UR1vRYQj6zHhsJSCTWssoz4FdeOjMQNUYQWBAEgeB5zIAcK%2BX20kuJIBCRpkG6313V9RU6QTfiYe5hFKbb%2FaSWig5uejGOnCjVGbZxhgG4%2FQilp%2FFd8hPsXgVM20ewMS%2FG1yk65TRZF8wOC4JA3Pro%2BVCd5&X-Amz-Signature=92a096afb0bbe60fcfe5dec31032ec8847d15865928bb19aeb6e8dc845d2c5a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

