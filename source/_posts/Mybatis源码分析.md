---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XT5WQ7PC%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T110044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGcwtBm4m2rzNxRTXmsa5a73bf5yICm8%2FeXJlCohyAe4AiEA8ewWGgSoUQ0Zcz21J6vurjg%2B63vc8%2FMG0cHOGqc0byEqiAQIvP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHxH3oBCpRDaDgCypCrcA6gHEESsMZhwh4b880uCF%2BIVAP2rAD1u%2F3kcUb%2FmRE3NP6LuY2RQaH8nExhAN%2Bo1iRUZcUFRF02SWpXDTZ%2FaC0gd5F16ruwQ5toNoCRwMWBARo9rYDood7l8LSUsRCwOFnO6Y4bDQ%2BdrvefWvT3YwdLZVP9M3cB27UBOy%2B6gzsfDNf57RUJ2ODya9p20jw4JRPfOPc2pn%2Fer%2BfVz6C5gcy%2FA976f9CyZIKUuvhhUtj3HhLe4a8ztt%2BsOciHGFcE8QPIgLIgj1xwUm%2FaiqlPZzovIAO7%2FOkLP0LXO9BPmNySS8EB4SlbS0JpcXzW3uTmlIUdgPexgPavFX4voXwQaAhLU62haiXbrgB5xiPVu3ReElN6OnD61A%2FQEaIPDMWfafhkEKLHq%2BWVWzGetbFBwwH8WbNZl73%2BsGsBtJY5h7bNZYJcO1Q6%2FIdJMuOtM7RccrKMFEuEOjRghvS30bIwvcBpDlKjAbSWXAUj7orja1fl7RM%2Bvaa7plmzcRZGakXi32fADMZ5WOHe837WHpIlOD78siPZNGZ%2Fc7hch3hXA1OfGSVUXWRtjqIDFBo0GBKeRet6Rwq3XBnE%2FygXKqPEtOkovxdm2H5cgiccNxFbzSeIa5yljRJ9%2BevLbZSbGMPmQt8gGOqUBuqJ%2BdNhvy0QDrkS2l0jucC626XA0Y6qEZSIrSUWdTM677HCL6nGTT4pLLNC6pqvMTyxuai3CPebioW0jMGzFsZ8RrMNKrJwYJ16GgbOViECbIa%2BTGxOXtoU8375tHlF1iZMIhW%2FGzm2epz7494I9m%2F%2FEU9LjF6YLkAOI3UbT8lnqwKRtnx0iFzax%2FYmsjW6y40tvTbX2TAKo%2F8A39Xs7nKFaN4pO&X-Amz-Signature=7ad7f814f7b48aa2448359e7db37f9dbf6a919040999f73dfe90d7652380e218&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

