---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665R5EUPBU%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDGvKslkBv2WhCdHwwvsVrd1VY6vQXpJrJe%2Fwq7gNhYbAIge%2BAWM3t9FGJTNUuFb%2FDFNVznBg6WZuSJGkPzJ7vaqecqiAQIhf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCmxtJsFs0Dbq0j9pyrcA2Y4kaX6WUedaoOKcbggh1%2F4uygOAnY9Xn0RGqAFcPTsDLpZnOcalIe%2FNXZIcGWEPEJ2JMpVFCl4vjNH0zD92RRedZQGYX2f8kRhC9UBVIpwlgnuFXxetJy35TPE2at2TJz8sFCh4wXrtwKbmy93iemp1CmLPMYKwFOSZ4NhFFNYUyDg%2FIu3swHjql7ypoWYt%2BZJME3xzh6SmYOu73Lx%2FYLP2M0MRyOzqBtJn9fKizrERNTBIYd3NwV5O7ABj0%2FDhueJp7W8JQZyqmRMkA2Xar3I0hbUelrNGsfkwcyExiSzTib89kBxNlduq0yo1RCIuxSvYUSpXa%2Fevj%2BteUddByuuZ0Rr%2FbuZXfMmvlyV%2FLSxnMMUtRsen5IkWBqhCFNaCVbCIbF8ZDvgrEp2zibCNOuP8T4Sy8MKFAvBAaxLWDlabtwI%2BKDqgwmHkmqJSf7MU4uY1A7J5uaVy%2BhxYWDRr3xfhRH6x%2BeTK2GlAyD6ICENmWi2IJVAKpyJCSgUvXlL0aydKYo9eVyduBMvxQKCcyoXLx3tJgMzmxxHLfFxpQ5wk6Rsry63IXcEAs%2FLhtM%2BTZEPEo%2FfzlAFpJ1uekjAO5nLhkaIvWDqU6Ez901D8J9YuxfkybVD0boptLVmMMzhwccGOqUBKiTVMO4GNFWnVyFVxhqcngH9tfn12ACGIYMsfInWA%2BhA2MmvIr3oBclBQPhSDFuS3GK3cjey7nszk2Rlj5ynyRI1Ln0twssJg3V5BhFNo9EdErP1yjZc7t2s2bUyW3wNCHi0qT%2Ba9IHHOfasaqZM3kWBcT5nYjgMz1S8zmSTcB6ai9vQftClSHSyOn0GV8oyi2MEJxg1q7DsPOzmqQosl6%2F5cHic&X-Amz-Signature=18854e469e53643617833dbcad12d9c72a236f203c83ad3eb116cac34b8053c1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

