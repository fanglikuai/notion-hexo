---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WG3XXE2S%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T140050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBGgLHWzYAICqDFX%2FewYprQg%2FjamiyVPiF82YlLlHGP2AiBEeS0YPfjFb0wwEo1Igm%2BAOTEjxcv0QlQMKXc6EgYxpSqIBAjF%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0h%2F3pSw8JwiBzdvLKtwDkFgXXm%2Bf%2FchblOn0TD%2FckxGdFkfz9ieNhu13LSs5JRI5iHsAzQNnUPfrs683ZzoDXe9q0eePwq6syTO8Ur%2BiZbRjTAvW%2FCM8CSD6uGk4ux06cBhow%2BzlRSaH6atiJgRG%2F59ZhlyBg3D%2B5uEqGIxxuxV63d6ZFJsaZ9CWaGmVnbs4NIJf%2B4PVH0pGi3uhTIV%2FSH2ZXqdlJtuNkmyK5tHLLw7jlHlsJFHAXpCgv1SNrBlXiPmRcVLhNekx6aSILeePYEGLlpMKk4ivPrR19OOndi%2FOLKBjdT1UqxBrhomqidfoiwYZm4ELTBnYZthv4FeqUVH3BfSUa2hOymEHRNSWTIQRyrgvjRJfR4qU8YJBCfqWzSsGDD5J3p88AOxz42PXSULq5tEVQPp4BS4CpiRGeavs0yg6WPQq9jt%2Fdh7jwVgQNohDJcpvYKk4DWA%2BTUBFxuihb7PuPUyCpixqSJMURAw2t2ppkoSZS7G0Uxlv2BFipB5UNcUWR9u5FxZrY9KLZ95tto0iUGCq53XEZUjdGUO9IxxZt9Jf1fSbflBcFFNesyHHkBzQlh1GU73Si0sDQStMYXGCdl5VL42OGvj7mFE21DdcdBKX5KO%2FbPjgKwsG9CrnMT9MwJzLokcw%2BsTxyAY6pgFyibE3vxdRjaUa6P%2B1%2FDajMnRBKAQbyuGKd8EVdhEAkpOHVeqh5bB3YCPPeH1fsttGpfhbflAKRgADqWGkyWS%2FLyQ1wx%2FeqXLeTVK%2FwsXlVCeUaOwqJMBxUEwTCamOt2oNBZg8b7PrzQWaX%2FY13%2BJ7rjGW6JFZqk4BT%2B7v8mV0PLjBokTR1MI1jpI4eZCUUc3suHijiHiv6uABwqc1LEoT6v4zjBTf&X-Amz-Signature=50bebd37d75f967509873f5e0f92d311ae65a6eb77d1cfc06b2290aebf5e2e81&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

