---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SUHKCC7L%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T070043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvX%2BXPreYLbTcVMZbZ90XiTF32JMtV%2BAjggmGO6AE83AIgOM9l9jCDXFcPaBxaHkqUDrK%2Bj1LfIe3Q5VjPJQjRAtUqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO4IfN1PzeCMFPQM0CrcAzUOghLBdZiWwBnBpyp8368LAcsEjhSafXtsjYbIqWk0swly79U%2F2b2UPCMOvoQex1doEKfCoHiNt0JDjeVKx%2F%2FkzbMqC0Rw8IqfTgNZ8M9rXT0oCz0B%2BcR8MoOc2%2FWIcA4VhgNQFoghKutDu31qC34HbHdvtK%2Fvrr8PkgRPdNSjQhYgmVu7e3hSOFNA2WDUk9vNZfdxRaWiqgHmEHuuDMBpy9j6%2FaVv1htkxDkAnOKOgORnaVyDa7aRyUvWPamvMmy8kwExwqfCqbrCJRBeLUW1p4dO4JlW4tK%2Bv3s%2FGK0VnnVIKSPI78nW9QzdTp%2F%2FatUUEQhikN6rnGiG258KxM0WlfZHZ2abMjM2GbLvn4CUC4q7FknT3yWIrh8SG4eLdsvGiJ0TTG%2FchBHrUpMX%2BB28O1lLWsJatEmZ7F%2FuwxjbtjEeDnzgzZ2IMDFZItD8N27bi%2FzFxWyHTo9%2F0HMpf1Rme3hr2nNvsplqH6R%2BJ9uPT%2FEfSQ46MFUm5trJNd3TGkcNEf5cJ%2FoRmbJVMB2Q2psbxZjvIx7CGMYj2HdMO6kr50URLUmVjNeaF3v%2Bmvk%2BGpG7D3Rp8V7q%2Bz4ptJDLHqywpb7%2Fgu7UGWq5MWPcaqZ6PaWYWF5fsu%2FpO5lcMKe6sMgGOqUBOJsN19%2FNn1BXG%2BxCoinvWd%2F%2FjKPNzBbBXfNlnnC%2FpJGKc6NNRVW9aECnEXjmCl5U33VXwM56azk%2FXhB1%2BOYWLmNpdwbNVdoyjaZOZ62GYX9Cc8ARQ%2BHMS2tUi4YclB9tAH6JzlRbd3zomXGwB%2BTMWRQJG4RdlFMZluOzIW5RPxrlyL8%2BSBaeBhHZ9KJAqkUvjnMiyj7Tqn%2FyVjV%2FwBsSKdEVnLsz&X-Amz-Signature=9b99681a1640ccc3a1bbb5257e2587b895dbfe5afd852225fa3d34269a101eeb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

