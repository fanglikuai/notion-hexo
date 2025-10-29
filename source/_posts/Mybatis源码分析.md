---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF7CMSC5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIEXGsgrS7U3j53QIHY0FXHR6oUTYBedrbZxTAnWat9MPAiEAjPfDzCARxtX2WFVL3IfR6wVCvacPLRyHmyKO2BYrGg0qiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI5BT8tIHjXFQRiXYyrcA6VPgBeOab2rGPQ9KsqqVH4ilmYNNJ8ZOWNmLpWEdAeQr2MDZlPdoA5zaWpTOHjrBJmC5ZaCHdHCdAqM%2Fl134AAqAQeOMFuaeNiu8hXJ1IXh9IbQa6eG22guzfNZnDwnodyT0Qcd9QqNhTZIqBfrcIEDxZyRvo7aCsdLN2ILOFwsf0oo3SydYNsFqBZ5xDxVF3vDA49%2BnXd1y7P71VeDw5X95PAUgQJjlVDgmyWFfwMiXc38D08A0RPGTIRExV0wyKaS9dcg682lh25EgLqAIbsFIWPV3lSuYRHs%2FXRRLepKRNXBEuj3ISd02W9Dsc68%2FS60bzdBqhYSX4tcHbxHESw9dP5oMjp%2BZwTIpJJJYowW21Q9C%2BtM%2F3SUr2SLp7e%2Fip%2FLiJaeTjkH%2B6BoI%2F7zHJPegQYcgeIvwwLPrf7nMh2S6GByDz4zusNcva8sCn3e%2BSiVamzHMQsBgMBElwGeAGzGThnu3g6019voC3WtqYGNIm2kOXC6rZ%2Byip8qQNGcd%2FiIcN1%2BkMCLF%2BS9JmBgGhfhENBxn3AUJrwFSLZJYksAlwoFBnIOnhLJRbl%2BMYBhCA%2BouAj1onZSbRvd8on93u3Sus71%2F63VVdKsiwoGc0IbpyofmikBfMeEUDQRMPPTiMgGOqUBj8ca4kbdElawkmO%2Ff0eyCSRiL5eawlAJvejUxEWW6nwREiYgWSu4fazCmxbFWBPxRGUKT9yyPuXdThuauRTGS4JM3zH6mTsBANj%2B2A7RqNV2zCDb5r1Q2mvC66eZPR%2B60hFJbJioIbgaglgqaYSvp6Uf6l3EKPkmDnwOVy0la7QdkcAOIPoSn%2F%2F%2BlY7MonU9lie8nzmFNLjeIBsXwKWqGmA0snbL&X-Amz-Signature=c21076b09763fd91fc0089e2ed7b08033ca654029fcb3021a9f6d8753ea0952f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

