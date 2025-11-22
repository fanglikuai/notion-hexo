---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666244QSPF%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T200045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQDI1Pw0HaS6qItQmr4uhXlhbDHBXAt9i0nqAulyAVuZUQIgK6zh0eZARwhmsg07TeVxW2hYfh6A%2B7RcqhDJx9WkTw4q%2FwMILRAAGgw2Mzc0MjMxODM4MDUiDAWFS4kMsrEh28b9gyrcAzGZa1y0UvQanfiVI6Ium02HMcSBDS%2Fhunn%2Bd8StGB34CyteJjIYkALzxvuNxCYPBfWOLjU%2B0rmL7obvBABdFnpk301WM09Dx%2B1D7I%2BsdqwcqjxTFJhHqcjW1E8EZv8Vo5snj7ivRgfv39Ixkhv6rQTX7DTtbgV217iVA49PZjxom%2Bn7%2BUGgcpMbVsVXWSC9gANcOmsUGGHKG8K0K0JAYGuZ78W7zLztABLoxsExz0HnM6P0u6H8XM3eZK6yd9eXI1wunjLN%2Fqrb5c4FKNksW16n5LRLBN6aXTM6IpT6Ft7uTk59od7FrWVrRcoZFKIX%2BMZezN03SyfOi5Oq%2BDmib9ArvJCTpyCjXj1Qa%2By4K3YqRDFNBUIFotHBDH0szA85R5g4Fwxnm7sM8s1GjdnZtDk%2B7RQvghvX%2Fe9wh6%2F4Aoq2HNQxFJm4AIHcZ%2F6FV9DHHXF3EGTzZflOsb1a2FFPp132GeZ8uo1HXk1bVM%2FOIZ1MUO%2B81kcI37OWPQ2tlgWoRoDNx%2FsnTEgmMFCW3hFA6Ksz9wKUb5RkuPda2kd1JQixn1%2Bql3ll5Mr3eKWTC1LHLpQ48F0OrQSdKaJD8LG0wRuxPS6pSon3vkBLuL8uOd5rlkDnMx7v3aRErs4bMPKjiMkGOqUBURy3o%2FojaFOGGyVvmRONMCWgSzGxLblyG03CiW0cpeNDaIRJYsk%2BbqewicTYpP%2BWjxM%2Ftuu4aqxkCLmzYh%2BFkTFy9aLv7PeXvHDn9QnNLAYUqNNjotPtqtoNx08KvY5x8EQG6zIt3CpbzRcNvzStlOxtgYSk5ecgQFSez2LAPF6HQ7KqDVNJjQC8m8yzashAFrLkcdFGpldSEmcdqYhp3r%2BoHarr&X-Amz-Signature=4a34ab0c2528d5d88f0b9420539189be1c3d1d242f17facaa0579df09d1dd024&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

