---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UMSQWIDF%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T150058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEG0aCXVzLXdlc3QtMiJHMEUCIQDtftWTgUGJA8PY57CVOscx%2FTPeMv7u2smX74hlywN%2BrQIgPmVjyIGtTI4n%2BCP%2FQ%2BZI200zOHp7lmmLkWZl6986sksq%2FwMIFhAAGgw2Mzc0MjMxODM4MDUiDE8U%2FTX8zJFZTNjHeSrcA1SmpUzzAAz76hUTQ1wLw4YLWus1pe%2FLDHuJ70U%2BhCnFswzRsmi4lD2sj9X8PY1uob0Cp289yYY2gIp1C2PV67G%2FACQza%2FlPYY1zn9pLMcp%2FU5sy6hyMZ6F5UU4rateD%2FXKsM8VEXmfL%2B8wk4zobioSkOo6wJ%2F18TEUImvLRs5fjIv3N43iT4aOJ2jREnEVuk9qbZ7z2CWJZye37xJ64wY%2FfEZ6eu5tCv7SmiwPxdGzRPyWrcrtRzWwC7XEHNnZym4JxLQTzBB9ua4MAIJomiWv9XBWzuHwkOr0l8ECEEEjzFGH9dRAf5BcIIkC7RmUGdcaa%2FCbt91jGNrRFIngMJ5AWeFd5cobqWsA2cbxxN3Z3CLePcVRj%2BrKYiKA7veJERnbDLgLViVXDIp6xK5yG%2B2AxnUef9j4VsIdNUCFAHO%2BXldlinvVZnkzO1uXIhgXgSdB13L4l3BojHm2Lr7XzgcFy4RfAqnFrki7%2FmVXnNHnDBsaKWxQJZUCxm8TkOsuPaKSlTTZ3qQaw16YPhv1ZicdQyViRiFsRuc1xIx1gHQ2G3tgRR5TGkR0sfo1ge%2BrfQ8Cf4mduGMKy9%2BimmpzD%2BdkGWuOkqx68Z0Yp0aMMJni2PJHzr0AhwCyyQ%2F8XMNGkqccGOqUBtprjTd5zks%2FRsQhxDduAUWKSHIDg%2F%2FUm0SZH5VqcdAhABY9rGgg06H5bgx0dyHWBJbCYv4sFml5fpqmJWEUmcsCDPlTfMmyRLk%2FkoHRSl612e%2BMi2u6YEgdOkGrFhoZh5F2QGvJouUntsyzXpoqm%2FBq6%2FHjLoTLl0Ph02IjPRteprR3vonyJuZ94WebYbJwJ%2BTmdep1orQgju%2BHsmECMATjyELeF&X-Amz-Signature=f9cfc63760fd7f656c8c54d3f5b1bf768eef7e318a0d05e59993ab290798f1ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

