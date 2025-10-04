---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZY7OX6NL%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDrnzEATG9VfxwANfsVdM82NA%2FkjzUbWpMwEkfDB50SPwIgGuIUYktxdFUF2V%2BaD1jRPO%2Fi3VM03INCB9TsoX64xNQq%2FwMIUxAAGgw2Mzc0MjMxODM4MDUiDNbWlealqu3jSoyLLyrcA4WjlcFUKSbAEHYuFbRBifo3EabJjW0L8yl9hTqp3NYJ8I1TCPYBg1aCqTePNhD5JnaaMy0Q7Op79kXH64RDKJiVP%2BdKWsvjholGsrxBv0pGV4Wb%2F2S5OG2UtVsALaH%2BHGvcC4JEChbnUH7qfnxoJmhaNChAAV4g2akf21fpCZzqGrdm8KrJuyPw%2Bv3HYyNNxf9np0FkdOyRB%2B%2FI0DazHC0ZNzCAm1CSL%2Fx81rUwJlWuUPGlKeF8PbZ0gbXWRW5WR8LX9bgPyMAkjCiaZRgQLw0q%2FO%2BX0z26D28%2FRErIq0KHfIpICmjFtsLR0qgZ327oB6easn6P%2F65dGb6dpxWSIbVojo%2FncVXBIJXv%2F4W1tWXCgic2dCh6db37EHSDFzK5Acsz4btvapIw4fP3vU4kLHUg%2FasV%2FUdRwcxsD2mc5YUuA7Hru7%2B4JpCEFONvNdhl6d8%2FChjb0kMIoim3DoiKOCNrFU9h%2BJ4TrFdRTD8VZQ%2FTKKtP5RvsTMT4%2Bcg1JjieTp8xdI78w6VslikUREQaTFJod9kjtEqv4RLPh1MErSnlafROPG5gYooVu7tF5SyOc6e6%2BbjaJ750eNNFoxtCg5RlKfqcpu6Je5qpBlEWhSbxTabCeQxGYHEhHamAMPb%2FgccGOqUBhpuERkZO2ZbPi%2FMitA3R%2FVWkr7DGBuCiG9TOZB7M9jIa1hMOs0oi%2FPMLQDoiutSUR%2Flc2qBN2mC3rs7VPb3c1hAqD4vqtaavFyqpD0or7OYlNf2FgB3QggXhdPX04NsdtsZKsve%2BnUHTpjyOF%2FDLHaeLBhZxTRerrI9D%2BabLoLEhhBiXMm%2BW3SnYNS4MFBKgAdA1l7ZAO8h11VXZxSjsf96WGyUt&X-Amz-Signature=92381500a0854a496799c5d046ddd1ee364c2b0e9150aa6299892137fa81c0a6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

