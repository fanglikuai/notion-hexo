---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RUIFQYVB%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T070040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBYKV3zvaEzQT1lHKcaHaFXafDm5vKfiAWYcBB1iJ1SeAiEAg7oiMqw0GHsYY4QVC7EPjq8UuMfIz3hCR%2Fb65pUEazEqiAQIh%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPvZD5oOu%2Bs1OsEU9ircAygD3%2BnEVORlh4wP4RE17evRzhCpufXJPsPDUDnscnKb1dPK97kPlR8ReM%2B0AXuVkmKo0Mgq3M%2FzihimxKNWZYJLbmQFpZo47H8tgfp8VtHGsjP8pEIZJxcaUYA9FbxRplC%2Frr9kZ%2FFAPEyBULsYsz8045f%2F3OLMXWx%2F%2BGiY2UXIqfEbmXJ3EH3zgiN8QqUY%2B1BLH%2By0CEHdKJslu0y3pschGmpO8%2BG2woHvDt2KJLFVvs8EmGrKNVUQRwMRMMtheLSZBhvB%2FS1oy1J5kk%2BGOpES6ZGIMXYp1zcNQYZBDTQEilJAEHzKMCCSfrZrqPS0qBwLZcxf2bnbD8cZOqyG9h1qC%2BeFc6Ox%2Fkf5%2BijkIRcIHOg%2BBjwiojzLfpSbN%2FUQ8CQLhaeVbg640stiENjH%2BDGXmIvo%2FrxV%2FzH6I9y7URt14G8rpUM4Yz7G3sKe5aynugGWSSQyzCip1y2OOqlccqQXoKPR0bKA4ZDoqW0ZUV5qfw2pv30Uhr5zRYm15cmzky7c0q0pvTqlmHWEXdwf4JfERHhA19n9hDnUm1COg3kWQIhXXouSopeaku6ttSNsVVCfvo3zj2d4zIcgcPJHx2YVzLfYJmTCreB7yRrAxnVNuFLOrKNMal9%2BTswsMLCNwscGOqUBm98vdtqBwS%2F3Ntsj8Hnkcv2EdHLAYiAyl%2FVeqW1GUFZp4gmeLwsR%2FaVrNd7SBlhYNiPmzhBPAasy7FLRmW578j2NC4bjcOCLOFGvVOMe1eXPL5S5py2s9n9Ccgj1v5XVCw5v3ZWIKI37dZ8TlfbEkt%2Ff%2B5nGix86BB79hW2xMtmAtTn%2FNu2zwuEVFxPaLqDii5MaexKkpx%2BGQ8iDYbdQ6nrO4W1Z&X-Amz-Signature=5df8e1df1d06557a72053e44a4522ff2ac7ddd6209b0962a91dc731fda461646&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

