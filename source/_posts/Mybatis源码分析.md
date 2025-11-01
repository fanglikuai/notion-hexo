---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662IZJHYXM%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T160044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQC3o4eBw0ADH5wOHuwwAtwR0VNl8XZ2aGXM1i7O9Bw51QIgVaswmA6PsDHISm%2BNJtu1Sg6JwxmTbkS0oR9vHhUgthsq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDLnCfo6UfnVyszG4nSrcA2qx66I%2F1ILaF4xMm43FyFaAKdjMELSaa3bF6Qscj5qVlHJ1PEmdVLfZmQb8m%2B77FkKoZaV7fJ6yhyQSDsAOQUoJriTh%2Fqm6sgCBZ4UmD2Qnfqn0nqzjvnZdRqqM6z6EncWFehP7x0i%2B7c0XnQpjzPgI37ZevE4hYbDq4SiOoPg%2FwFTselfbA1Czz0Tmjxd00CgGoAEe94ialBQRa0oPrg5oww19GaI094qgcg%2Fy1E37uoN0m65JFAS9NJVgUcLJfyKMZz0psP0OTI3LHlffshfaRntQNXLnvMcgPNbDatHL8lHNNN8hthAoTJUbzKSnxIeKPU%2BskNeR%2FifKsoVlMnsDhHzQBE6T7J%2FtyJiaJoP6UyHo3A1CxM27fShkXKAeoHSuJl2P%2BW%2FAhP%2BBYVcrJ26G40vA%2Bjj%2B0KeylYYql%2FFKsJpQVQ342dEDDAzzR2CmIuGrzF1NPjZWr0rbQHpzKf0xJHZHBPKYYXVKosrxtmZLgjzbWTNRZd2C2OPXamucQtorQ%2F4Grqzjw5aCV2jqyN4zdhnbF7tIgboflnCl3uJKQ2SgadVeTym%2FlcdXJXazSN8cnyMx9yyFG2cqVbKmVRi5L%2F89xc1hZu4TQHMxc4by%2BGHoXga2mq5weHIBMIDQmMgGOqUBIyer2v3Dw576I3KSIWLZSAHUXOSTM5g11Rq7eJv9xd%2FIC5GsxlEqDOhF6ADgdbDG9pa7PmwtV2GeYCbfNxtK6%2Bs5HwITB854zz1Mng9mlTD9RQtH3VO%2FDNex6DMw17moJFla1atQ1UdCD0ZAgpW8t8oEaehSq5FENRCZ3WGlWzqc9CCo66IDo72u8IyuMVH6rslhYjHFv94CAtu3Sc%2BcUwCwfVkL&X-Amz-Signature=f5358328301ab33b0e1aae1ed22a697cc1c2a1c6728ffb5e3b1c34611cdc9266&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

