---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Y3T6PYU%2F20251112%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251112T070045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJGMEQCIB9CgZh3H0r83p%2BOMC%2Fd1Cu9xoU2ALwEl6psc4eW%2Ft82AiBRhZmW4QD2OvJin81ht2rpt4DHANbVpC8SucKw9BBtvir%2FAwgwEAAaDDYzNzQyMzE4MzgwNSIMx4PLUWAw5lMh4OeLKtwDCK6%2Bx3ZlUykz1BXY6rreNE9%2FOTsebeHJwkoZVzY2OUyHXO84uOVabwI0SNeMN%2Fv3HD1KAvcDMFa2dv4cu4xb21aiFI9PPHtJbhzvZSXiovIx%2Bdx7CoCP3zldSm35zPTMhry2Zuhs6JBVMu8BnsOYTd536yuKSGtbqNd6Yk8XwkL3cuSOuS9LhocRU28sKqS1NBWggRarmCMZslFzOEWhQJUmNpY1l2rfCOL7tEMKdJfOiXOIbxSs7oYCtgeu%2FD31Kz61ZfxQKSvV1TeIUMw7p23gxMiUXtg%2FE7AfqTfVNZtvwrlcQv0VFQ7%2F%2Buxrhw4Q7LC0ZIXxEKY1Hh2JUVTloSy3rXEkP%2FPZNpHes24kAc7Ps68we1zGH99dVbWU%2BiYcRupK2pmuXEmr%2Fhi5BZIMwze%2FwB10xiU2zn4WVSnfMx98leibcNG4AUMPJlcAs8rnqMEQvetqQ8nC9Xk8eNnxQg%2Bb9%2Fji8TRuwgYANRcBxOr2bbzBo21liWrxFMDuYnFRvUJidgKtO2Ygr1l8rDRJR4HWVyWPMqGAdi6yzKVyg9erZyskn9qU2gMimSWwhnLU6CNQUpj%2BidQoZSE60zK5b8RBifCMx%2BzMOFiwAVbBZrugH6Q1G9Peirwjwa0wldDQyAY6pgH44bPOCIVw9lbwYuLGX8hu9WFjTYelcRwL%2BUyLzmCu5%2FegSjW06qAvVqzjBXPtR8w%2BSQxmPZCCThrhZXMBwIPifq8PTCqgq6347wc4Z9tbwdSRRNgvgrZ6Wp1l0R6KaWV%2BAmzKDOCUOYZIEza%2BHcVUeu7cFNQvPMl6V4Km8PLdes1mkbEuaOEUhl3qQyZojRyJGTo4OmxjRl%2FPvJdkzLmU2b%2Bfkqwz&X-Amz-Signature=cc95d0c507362445b3d33e0afd308720d65d6cb6c474fe9d2853857f524862da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

