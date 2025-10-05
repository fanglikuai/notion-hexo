---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665RRA7IMS%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T080049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFEeiP3gumFtjXvlMyjO%2F1eTk4aociK875HLXKeui6oUAiEA7y1kiazSD2L25qmpmHlc3S3JX4qD3toZwAzzLKVT0fIq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDJ8P5u58mcYrLCl6NCrcA4zPTLoyJOzeehlNgq%2FS6CPy1sqZQfGmtvTw2Femf%2FzjwSqgdZgJIUNuOfOLl4lYGoEOr0vpe%2FPneKAziP4EBbvSeOMABtMba1iGEzkPPoHq%2BjpUIxcaa5LFOuGgeJv79INbb8QeX9GxItvKf20%2FqoqrcLMPwdkTZYqbMIsy14zeKUfV8kDh3WOnUM1n9opDCY0r3h7qOhe75M8NwCl%2Bgj8JrWS%2Ftp1PM18dKqW9zgeX27lY0ILyH4qsMERkikEN%2FhK97oSV2YlZ59dgeTIvShQMdXZ8FbRuR3brdhfvn5zYkqidHyuMb0j1b%2FEBeJnEWwDyaPoMyt9qWh5bTj7R3m3onoBua%2BLGBUDkW7MSHdczc1aKRWixHEHOOmfE9ouUrZKbzNYDdC%2FzMVNVuoryFBQRBhYj0Lb1HzINkrlWA3LPVVVgw13VySGJc%2BCr8JYNg0auFw7GCyvVUSRoDx2we%2BK6zEVuDv1Nk1zJWZWfPYMMdi61kitvA6Vy2iO9fBLuOlJYs3DcDIpNGVc8p6O%2FbcQj569rs7qPSKTvbltZOrpj3oTE6zyfYaaTGXe%2F8Bfaacjs%2F4ch%2B%2BAd16Oe86RazVYg4vSa0zGunBOtexHtC0zbakaUZiFIFnjdlxINMLeHiMcGOqUB1DsdJ9vTQIiZ60d8wkJg6fT7T38EzeeHKkGyanT%2FKLlXlJFbLlUhMojhbPD8xHSX%2BMSPZ1BPteTWnVtPFtgkD7f3rkbT3hH1kyEbnsVCl0pB%2FK%2F0DS2Hs2eSjufMUGiCb5zOyh4qqsBymjqi4tOjIIT6QLhViBt4nqvb0rLdCJlMbuf8vBnvSYnQaJptcOrz7%2FwJ5NkQB9LrescKA6Q5z69hOEiy&X-Amz-Signature=b9413d13e68b8f58226580aca82e38be2c24d974b5da098cbcb7e449237dff54&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

