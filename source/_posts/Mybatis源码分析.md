---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664OMN2CBK%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T210042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC8TSlDbJtpa58omMHsbMPKoUQlEVk0qO7Z9VZ8Qxid%2FgIhAMRf94t8XeUifryoCnXFEQC4XSH7k4eHoulFh7A1m9CZKv8DCE4QABoMNjM3NDIzMTgzODA1IgxWhEdYUW2wbPDy1Poq3ANodHqO0qhlxlx69RwHiX1K01Cu1fORnnrzNIVz43exc%2FOPB65Fu1zHE864cIC%2FPwP913e%2FT9BbelxBz%2FO1xIJdxk19ckZM8G%2FktgMSf11c811O3nVvs65iNq5yGGuGxCPuLWnNngJcSjQHIGTc5Eb9E%2BKFVkycXnPxtoDQnn2CZSHLbMqJHs8BagJIjqQuZ4K9ErDWR%2BlsThYwuG2sXb66t0h7cenboQOFGqDpiLFuDxsYRVaqJ57gTwUKy0Xlya7Z%2BcNGbk6HnX7kBVmfFMEMAO8fQ7xXemcwqRxx%2FlDJeym4ScW6mTZU0O6XzMJ455IAX%2Buq%2F5Askukqr1DYpmYvLqgQDvODPTJzip6x3vRCeOxWc8WAe2ovUvWaiHSySCLzEEvPGwjRo4KGRxZDokcxBrndTptFzVR%2FCbCUVQMx1fCnyjDbmLJA05d5QuxxgU1zxzPXty0h6RXUnn5yjiKWNckkyt6APRPFcAj6NH5%2BloOnupeXkqaqhf4GcMlCyrIwGO6tSUTQBN%2BWC4KVFe0y9XXk7FU%2FVsvp2%2Fn4qnOZMq5rD1WGL87QZ1eJUOSuDrrK8%2B4prTUtI8umr0%2BZJGa2U8Rj%2Bm%2F2eY7SmZIqgWgaoP0bhxUy5eRgO%2Bw4fDCV%2Fp7IBjqkAet%2BmExmKQlnsrDVaHPHzuJvCrGHQlntNNFKk3GoSLVxV4fdDyN0tDD9kmV2oyr141%2BzKd9HoU0I2n%2F5vxBgrBdp27V1Yu016FMlZG6GOfPWBj9gIy6gLknRP4UjlPjn%2FQ57QnXwz7pi9syVtBcPRYb67kJje34WsASayVRRaJGWhvRWsnMtuMRFK3948Ee2Ljr3OiU6PAT2Lo3T4Zo3uf%2BNz9Q9&X-Amz-Signature=edaa5af9efc62ef0c31db1b65b69eb8a2335e18412b07fc596b41a233042e2a9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

