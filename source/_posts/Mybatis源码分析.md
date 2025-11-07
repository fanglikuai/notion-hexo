---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673BQDHJ5%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T190047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD3npDN6epBgnIvEkDqQrapMxg%2F609B8kBIw1VLyRRqZAIgGNAGXUGGPoiFA81PnrlqWDdj1wugOpsX%2BeVuzy8aCRUqiAQIxP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIFwFU%2FRfzME%2Bdr6jCrcA23c6YtiBlJZd1rTMouvSzmWyc5wDPAL3jijWGhFFDusHDjnRe4AIp9AfWljPoOmnfOHbPyS5B5T8LRr3gFuFBx1HGolce%2Fsca2hy1VkCaLPlasYSTGEo8ZWz8RHafrsAcygFMFC%2B9sQGpbgYV9Lzm1GXMhvupNC7b4tMCrLF6%2FCvkc2m6zo%2FKBkb5YDXSS3tiwnD0fTsT8dn18DQw1%2FNwOImXPNWkoflvsX98GE2WC9qkrDeTDa77KGx2bPK%2FtHYLYI633ntKnLDHQmHUhW%2FH76sclaCr4I2XBthYUeXh0CGZzp77KZEnrKd7%2BLVg7qnyFK0l7x7PbFK7a3Mpm50lGsvmasdpqonEopMqsvWpY7qbemSkr9Uf6L9LjfrmZ9EF7fT5t0Ave52lZxtc4hZqvXL%2FKGIj8JYKQ4HMVX7hCuIiX6dU2RJSA%2FA2vSu3eMcWiTafYZP3aZPSF9UG05JpnIr9sDUPjdKB6VT3biJ0KImuEu8MlpSiAF811RA9BRPcREuVwDojyjxsD3ZV%2BOCXYEom%2B9q5gmmoR1kSoMg%2FK3Qr99STwjPYbJIvj5583UI03ikSSZaIr6nKoF54Y3lKP0%2BYAnDjoPExCXIaf1%2FAw%2FNotnTHGeT25pGW2lMKX9uMgGOqUB8%2F68T5qclWUJWc%2FKm4DyLUPv3ar8HddebZ85KpQg24KejR9WFKfs%2F9kYFtCUrSlzs2rbCZLv6aKZ8OYc%2BUNwYEElSc%2F1KP0z%2FWoL5gnneFFKlYvGelFzxa%2BD5EIuiAFdaN5QqNUmYo5oSRjgcpMZo02appm8DA0pcqlaK%2FelDc%2B1sLaHZMCaiwQBOHEaJfrLg%2FbTywaMW%2Fn9ZXi5O5ih8PpoMQCw&X-Amz-Signature=e98f68136d56db0c8e22d5ddf3cd508cfe204278d7502cf6125c578814907f48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

