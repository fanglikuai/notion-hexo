---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6LEGFTD%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvUJhnbLvk3BjeHrzPf59%2FC25wEnT4zaOwCRR5NgLyQAIgQeWk3Fzv%2FvJxaJKSrVbChwagUOQthLhZqcurDChPTkYqiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKROUxgH1YdTRX0t5CrcA%2FqTeYg59YOoZwwHzFtk8w213IzBOYfC6cIZNgRc1rb2mkOUHpuD%2BPEWhtHnp4lCN1ap6vbN4no4fkQbxqapELcY7Cvm2FvKQacETVUwOtZuPwHrYIXXDSm996DpQnjcH5V9u7sQsg9EjfimWaYNfD6iNJ4R0vPrI59VYxS6By8WH5eikrxI6xPay3OH0obaJCCrEIF11OGBiyYuxBZzQl%2FH62egfy0u7qHyVQcx7DqVMDN9YZMNM9gsTauDptHXgtu%2FpLCRr5u0YPW%2FQQuFOz8g8seHghua0XeP7zltyd9%2FZHOaYWXqs%2F8OSvrT%2FTv9zKtg9CTXVckUkxN5xGuIp7czrcA2meEVEhl8RuOGJu0di%2Fwp4mIpDwxwtnVPPN97lZKhrqLZq3lfZ3ThaSbM%2B0Pa%2FXTCeTq6pEzJ6jrnvFTPZgfF8afMxEJqeDNEKoXPL0ddmytDKEWR%2BdSTAUt3Al7zogknVZAXGHxxfj0VJS7c3DPtDMy8Vjpxto%2FyeYAqIhmHvyox4Sn7KB%2FOAwQ12D9lJRMV%2F3NEFSXn21nsFWNDK3%2FOfom%2BXBFjLFAknxzTjwius6mlisSPI%2B6X1H2fY6wTm%2Bn3RVBpVlJvSXjzKYJG8savqobtwdkaXo6QMP795cgGOqUBQFfdXt1%2FnPj7yzZqbzHu9tJ4CEuj4PaYYBxHiZjhMuZKoABbPeZUQeUdf7SL1xurza1lTa4BEux03HSx0M2IrNMX2dMoCHLjLMQipUGDTZfrxtx%2F2YEj8FFZ419jqUzeeZfFgWE5oEIAZpUYcJxbXfL5raBV1VhAL2u88rqgtbJgm9ruMT523t7NKh4EOBYMpjP17nt4PggJKd6%2BCRfTLOKganiL&X-Amz-Signature=bdb67dbd20e48b742e9c6350a181a36c770eccaffefcaac6c0fba398d3f4028f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

