---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664X2CO2CL%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T040041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBIZu3VE9KuvP8M5k9H3gX%2F1xVWWdrK7hWDfswfuM7hVAiAXsnfdBXuAMLOBLpgvIIrOcfIYjmoo0r5UsW6NUbJnjyqIBAit%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMfrWvkCBVifEnx1KUKtwDETuTbWe7ENrBXHvHZFwVwps%2Bqe5urSrqzO2OUr266CpNO6wXkTglH%2BLhDdoi5YI6J2ZQQVZxrABs5INiL4TAuse5HhPleH6SvpZUZ550lmFCo73KXXix1T0Fk7%2BgilQRRCbd%2FXOAyBKjnGua4ZTXDaSChDyx2VbjqqGVmDqj%2F6vkxMMXzzXlOvpJpA6OvAKgsAZzRCZ2LGEZD1JEcvD5Az6pXWSDh2KcFE5TK0TFsqicQMbG44me2GpufZnMBYI3RCVCUjs0zKZZsRGLpN32SWTYg6jaVVuMgO46FubImP45cnar%2FgY4hcDMYg9ZSQUFxMiqr3yrA8BMn0F6L2KDpOho1b4gMEY%2BbXTaMb3uVS0rXqF28t58fgwJiwuR6MAfSYeToM78gHPjg%2FuN3p4GhMMknimHIVvzDOBMhOu3YHLi%2Ba3S2V5E43%2Fn7tS58EnD4yf1hClcRE6lFOviUK6QGyOtKdXxUv2dyaPCogQhi6mCVH8yCla2uBxezaS3hBVbLSvZUHSZo04Y7z2FSe4wBzMlm%2F93eUaRxwMsI2I5nf%2BiM3tgLnxG3%2BNHRryIQljW0ZqfM279KVnXUjcfBTzL2bDzb4sZOWaO1nTmTU424eedt6lJfWLlkqju1mswmrukyQY6pgGFFT2z8bn%2BFGfUJXCN%2BftPU018Du%2B9lFT0oSAP4IDiQk%2BXE%2BNvp3buwNIlwm%2Bb7cJuNiKhZIzwT784XAUWlZFe4N7%2FHoNMocSLbUmAMcHcHuzqlua0OyFZSu%2FWZc7EQiPrXGWjDR9oAKqFb4ap%2FE1Napa2nnjrdI4l8xljOlzXKOwQzenXpUv2zvBLrwp4Eb4hQ1rTapbe07u2jQmeRvEUVH57rjvS&X-Amz-Signature=dbf08fa3bdb61b67adf065a8807111a95dbda6d2078858006a805941ccd160ca&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

