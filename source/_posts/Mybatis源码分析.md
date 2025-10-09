---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665SQRAEA3%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDoaCXVzLXdlc3QtMiJHMEUCIQDxq5AGuBUkJxeTCIseg%2BMvBfs%2FEWYR%2BJSmhWCF8p0YlwIgR7TFCSUl5n%2BhvQtsrrN8PgeUvVaPK1ivFnBBRIj25xAqiAQI0%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHPU6klr5goaZqylMyrcA1m5Uv0DGkexYSnTwM4yS9STSEEbWqMyooq174T%2FA%2FtOBBslS0ulckyxxdoLaJbqIfJi0R%2FezTNHOAh7sV3e%2FPtVxSyUZpPhYU6UA6tOITMXhr3KLFIPcsS%2BccKbwP6ZVkovKKStR8lXl5iOuvKyhpZifmwWjdYhVmSfDL1z6Mc%2Fkt8SjB0vY3y%2FB2Gr%2B5W1hjY03MKDKPPHdmvHXXY%2Fv3uRuOQJGzu4a6OzTxontmdFsbxGQgKTWAER0vM0NFgzcbPGG2bySNSOKn2ZPBeob03Dz3a3ELQ5fbljSnW8O1Tn%2F3w8Wd06CsFv10KoUU8KAZFtyEbjyxwcGMZMz0AhOTXX8WirJUNFk3tfwS7rSn1vPBwIe6jpnJohXg3kFwQmY%2B0H8vINA5cBU7EtCZPADhWPJLemes8Bzli1kkeeuHUqyt6Y49Km0djmPHLZ5T6mKAHVtWDMg4cAbfu7VvMH0gKs5pgJ06FzX9HIh1GdbYslx4b4EWCGz9n5ZYg%2FJqbRs3zo56Tn78DrRM7cq36EkvDOWSlUSSSeomZRaVPIuMYTdnO2yy%2BEXuRdceuQhpsjgBuRx8QBokXY2hvINB%2FScdoBED2EdOv84JyZx56KVkAQq7hMCOsLJYvC4y3LMNyRnscGOqUBAVwwgAz2%2BtntnSayRbHRzr98ze27Tjss%2B8zPXoNDDsAUxc1%2FVfX%2F6hSjUXbalavjqEBEU0UMxtO3IvkRHqv2c4Nre%2FweXa92VLy26RCBNjEPvUSlDS5aYyBZ31XDenAuwa5B60jjbmRrjRLcPdNhWJ61dwQPFLc8zC7FIpK1astWRTj0Cu1Mru%2BsEM7Fy8eIMgiWB53VvPA4Br76FJh7ZfemTjeb&X-Amz-Signature=bc04e07e0c0ac4a4ad54a78a3d8863d5b28fd046c6d56a93f5f03876ab3764c6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

