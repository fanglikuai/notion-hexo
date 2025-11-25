---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643UVWU2D%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCV2W6HlFTt%2Bp7iFFG2lw21KXh1Bfe3dqTaG3%2FHY%2FdoAAIgE0TrxBimeSmGtp1Wocum3x%2BPPdHFROItCo8hKlsGU%2Fcq%2FwMIZBAAGgw2Mzc0MjMxODM4MDUiDLCsBMskkvxUDjnB%2BircAzgVpJIlxkv3%2BRnqNMCtDRC7sUBF4tI6JdruCVwyZXrLjmyNDY%2BfvFl2pzuP1109re9yp9K2k9hfxfZmbryRBPt8u8gDm1qMJUbfREX2Xm%2BOKSiUoe1KDeFW1XTVotaWgND%2B4JSjn1O%2BZVWNH7viV0dCvkeVnFdExZj0pwMGiv62QKrbQhnOFqyPekPGfgky4xZYjAAqKoMHHNKIbPhUHzPRmjijdA9oriWdhJnVhyiqkKUoGiTOeoOGOk8Ktw4CmGmoDZpCEaG3yIkduqw4yzBABLWHmFZwvhb2vgTGQoA8OEWTJIUVmrFz5vqEm37qt2gRwLo%2B87t242jkg6pMBNZi20A3Mb3zgK0Gwpiri5Rn1KI4pNGDzfhooPd4mQuDtmhUISg4ed6v0johJcN42UWUe%2B2viL9K5Y877xErdLPp3i5UBrGVMER4rjHZlmU%2BHA90sAK2UhbHEZUbh7VGd%2FTqmbT4XTPt4r1vJFOf74zlc0n0u5LMASlWbFTWV5ZnQQnZuiI2IJ9hhjhxVJctnPwuqqf%2FvWVMeRKbWWqlaiDra9fJTsObDz%2ByXZwAil2a40wUXtM6tzVB%2BgE2Y4x1c0KG%2FdECPBN%2FEvnFGsE7KMOjGh0Ez5ZfWoTuGkX4MKS5lMkGOqUBZYDYoarproLna%2Bi%2FVbIFTiEyEi7ENvHMIfelUo%2BhdGsNduAhdlYvioLyei%2B3s%2Fe2LYgWthTJpRMUvqehRKm4JFKa0%2F9JUzxYfLdpUc00fuwBNY6Ij%2BKYa3SEkCqP%2BPzv4LTFYbCxjscOqyHr0SRa1pIUm8GZkouws%2BtrHG%2B5xEJv41iYUkC4J8pZcwF7lKvDeYKYUGhDGRjIRIb9yuF4K7rDKCAj&X-Amz-Signature=7c1efa8b21e9948a6545ae97e753880e944abcaa496ff099b04e974473d527fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

