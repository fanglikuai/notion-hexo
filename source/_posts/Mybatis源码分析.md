---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XEJ2QJTL%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T080052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3L7%2B71515DZxueFKLhNE8F0ROeGXXwZh%2FC2MG0NveNwIgKTXwF8Kj1wBP8FQyei1zZSiUDQpRYzcgjaYxlmGumYkqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDL7jnbKe9m4cCWsNEyrcA%2B3W4OVrbr%2FzirVjweOZpMNbDz2madc8aZEKX5LsANC6wpLd1o9uMHZAdPa4nHsWrB5%2Bz3tNLxAXntwuX%2FBJ46KPc7dgXsLbMS%2Bb2yrYHfhPWCtEisH%2FMjnda7okaDY%2B4A77BKaRTpQWq04M7R2mSgB4PTUSzAiO5cvgS%2Fm1uSSrxhAt3%2FZzXbJqXhbaqJZSq%2Bp6ysGeGcTaK0ti0PXH9FXeaX8fSFzR1GqeLONqNJqP2luSMlPmiUlstb83Ww3u2sHEyGaQePEPvddSn4jbXgs9HmuU8iYybJpTWf6T0ppKnPfMSIyjOs%2BeO1sK3mXSNbJpH80V876gZ%2Bxm%2Bbu8KgEISUfimgZRdGNTwCD8zxECk4FJHIew6lYBq9SXZ15NXc3%2FBSxIxHfzF7N8ONkcbZzHFb%2BKXk0KqUpYmtrl7W33V0y7DlxeO34IpJaIYCqgv0GfNcGYiZOpo8zcXuseouftCoJzeBUhPEFDixROfrdE7FaWn9hF4WoT1q4gYSxoMutyNS0KGY5OtCas7l%2FjuJb3d0%2FpROwdKk7qxcRwuG6mLvuiwWEHINzlhSW3cmO88akcK71VINlfODIidnI0RtjYV3ISQyFIu0Ur8xFSozfJFD0nKIGALgL%2FHf2fMNSFrMgGOqUB9yTTAEQEwBpob9pMmWuuuHRTOPpneXgdbU120xrU%2FYeLTebcQ8V4GdZ%2Fj%2F2wLSADP5gSGeuZUQkHRUb73j4fuLqVBpEnL7wovXxoxuvqsPwcakMamDeTHEGNoEVANX7f%2FS%2FXHi1ajM2XjlZVDmV2IH7bJKNHHsrZphhVGPStpDfnt%2FY4qh%2BomXI3ziuxq4%2FxSsw52ZTAHY%2BfTLGjn6tonGJmQY6u&X-Amz-Signature=7322849ceafa9d5217e2bd7deff44f7bf72c3b74db71bdf64725566058bd9c5d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

