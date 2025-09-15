---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662UEE7AHP%2F20250915%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250915T142021Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAUE0XQL%2BZOrHEcg2IxTWDz41IlSOOeH6TIj%2BJaj%2FGyRAiEA4lfmJp2OUatNeduasTbsRwQNFBA4%2FBmTmQE6m3OkWQQq%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDGz%2F4l3OFeFeu70zJyrcAwtrlgikzcImovJfPeCYN31MvTpValdgTcjCydPWTQOlvPnTDR5CmAzm9LoYA2XpwI0UDsnvsJiBBxgtB%2FaXl2j5wam9yR6%2BCKPnFIrks%2Fs47dF8eMUrEltFkFJFkraNQTkkeME2Ddbd6T6NQQ94N8vFPV4tmnwGPxckBfe4mIBq27dyYUrOxkSgIXkcegOJh2NY3h8GYdqRISXQSrYtjJz2q%2FS%2BLcYFyj3flw0iGnM5%2FUHsIelnSfBJ93sOPAeeshddheT%2B2BKmsP%2F8r0fY%2FdV36rm2EoWDkchSKGata4oKQSyhGKZvbbWBS9abSJ5sYHsHtEBw5Y2OovDLmnwHW2M3MAnIGLvr7ONvf%2BHR5tYH0Sr5BLyItT7pe1NJT6W9qLUD7Rsaeq7TkcV2Botea5qJv7aScnASw1%2FzmFu2KNCZXjmlT7%2FH6EJHFMw3QAKrIJmVDE9VzqooSRUXiZtRvX4qeg4S5Q1Cl7hfTdqKm%2Fqm%2FmktXuvvcjZIsgSC92FIJmJchEfR4eZlXJ%2FFAnH2rE6MHK4utPttm0mSA8gqsYiV3Vt40d7cd064wG8qGWG0jcjxeLuaYuKoUrkAxMTqlgf1dtWH%2BiEl%2BzbOVZOnkPmWTSmW4v6tsiGPGADFMO61oMYGOqUBkfeNxqvZ0p2b%2F9HVs9elMMe0IB%2FiNcAtu4R%2FC00%2BomoS12HIKpA7cUu8RogZf8r8bO%2FShUB1kDJIUlNOURbo8onzBBru7UC5olU1x04WaLiTsiuJBwTMRNXEEuww44Pl9FBUz4ssxmHQp1C26NbYZcmATB%2Bs6BjuNpQy%2FxQhEiuGsBHA%2Bf8RbvOjHTEBs6s3BNXmBjC2J%2FJvc1zpVJ5HR4LnnOrW&X-Amz-Signature=cff2c537eb4e7cdc595292b50e4f7f829b0b065b3596b303a53841d52aec9c6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-15 09:56:00'
index_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
banner_img: /images/0a4c2b7f4d2d770dcdd8d10424cc4b94.png
---

# 实例


```java
public class Main {    public static void main(String[] args) throws IOException {        //1.读取配置文件        InputStream in = Resources.getResourceAsStream("mybatis-config.xml");        //2.创建SqlSessionFactory工厂        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();        SqlSessionFactory factory = builder.build(in);        //3.使用工厂生产SqlSession对象        SqlSession session = factory.openSession();        //4.使用SqlSession创建Dao接口的代理对象        IUserDao userDao = session.getMapper(IUserDao.class);        //5.使用代理对象执行方法        List<User> users = userDao.findAll();        for (User user : users) {            System.out.println(user);        }        //6.释放资源        session.close();        in.close();    }}
```


# 步骤分析

1. 解析xml配置

寄了 太底层了


# 附录


## objectFactory

