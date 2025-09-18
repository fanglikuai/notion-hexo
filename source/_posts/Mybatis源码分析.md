---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7JJKXOB%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEkaCXVzLXdlc3QtMiJHMEUCIChwcraQVZ0ojuyvhSRwEiEoAdzMH4dBdy9P95J%2FmtN1AiEAsrLVHOYxFUid2m2r4deBydHTri4QsxQtOy%2B0rKeuSuYqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDClW9bVLGD6OSSh4WCrcA7wWFuW%2Bye448ODb2nFLqE%2FYoz%2Fs6x9IOvQVmuNeaOI%2F%2BDmE3oizXfGt5sycloXyCNBki6Ow5EfQhIdUZyEb4RGpz63KwljDHDc5jFiVsYT70G%2BEacQ3%2Bov5PQegWOKi5%2B8XXdVNrBSdCqZxFS5SzpwMnY0F4X1E5WWwktRUCJPvYBrq5b8XnwPLjXJVTiPFEIUc4fa1sZNJ42R0TLFXMj5RTVWwas4f3ijDGKWNWkqLUHUipnakpaHroyGwlCv89WcPGa52uBCA0hxQiNrClNQk5tNH9V1Viz%2BYp848LZ8oJHqKXFw3pRypuRsohYbRE6BPn3F2NaOr97V%2BdSZ%2BRcFm3W87WKxVBpuz%2BpnPmRkwOT0Y2G4mpe2QVhLm9MW1RCUlN4zllkse1%2FeEKyFDYtRHAEqDLrsuuv5lazaihhPG2Ylj5DmEmNWiJTH7RmAv3LgAUkVuq9gBiz3vAyTPDGgLOa1PStXizIBSYdMROYNwKoVxrFXyNkonLf25iNjqbpnKpCKiKbdap1O2Qshe7dVql8q9WDMqZjJa4q8cQIpQ3pipoHed8zrJttLhpayt%2BLVJIUWHJ8N7NiIB%2BRa304990Lw2usFw6y7HfMZudCiDp4LhKDhEx4pnMLePML35sMYGOqUBxE27i7lcSItPPnbV02YJdrC6UBzbMKvvR9SWKUEKonoPC1H%2FoapGKCbXW3iXkWwq6Fj6qum0btfqtF8mSvipHorxiDvxWKei2rWFZqRa1Y32nGhB6fNLDXPOMyOQsIs0zq4DpXBM6kL7J4D89lYRYhXV%2BUT%2BbIPmWzfInzhNNOxjJpkcM%2B07nZrhy1SUOsb22T1h%2FabwefboLWkEQ2ZmJQjhXzkQ&X-Amz-Signature=ce0483b845cfb381014066ef44a5ea77d4025a9bdd836736cec99ab6f15dc0db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

