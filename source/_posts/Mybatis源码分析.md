---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YLE2HJ22%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T070057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH%2FtVPW5OzXspprQBLZqnXZCZm0kiGDQ2gvhARZyZ8RRAiA8Nno%2BSUs7GEsiO%2FRRUnjJZQzpzLIDvazqao27QJ4F4CqIBAjA%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM1CCF6wfgmRQPN9lJKtwDbgg2hhMRv0YBGoKLlDDtZJgtgxyeO2uwPCmSx9Ius1Js2uAP2Lw8ttZkewIKCn3eo5MMgoBOEueVGZm1xHqMcigcYJzuHUjgfFdVCns%2Bs%2BoQAua6V1wnMOJwiSLt3hZMtAGmfRXhixktnYkG4FOQAVB4rLgaoQT3DxpLFSU2Y2C8FaVEetEsArYapLQyG4fbLPqogi13AgTWW9sH%2B1xWzB8gzkENahf%2F8JZmooM61PQIjxlvQZ88LnwJlGGoVYsx%2F6ABv5v72vHswGKxOlTYZtRiTme27VvTzCMEMrwxXLxhg9NgV0j02z2djiLD7nnFwEouLlMyTQhGEoTf1V6oGOXWlru%2Bde%2B9wH6RGbxiF88n06k1FvzEaSDSOIXYblrzmmCD2gawCI3oRHWOcC612zR%2FT4IsADVfIUhB%2F5QTZ4fj2P7ncb9bSZB0c8FWrrwN%2BCyFqAljTwjdbL69gFTpunzgaz4tXdeegO0f97tW58NsIhYtsXona30Y%2BAmf8BYsnXK2ebP%2FnP4haE3vxa2sSFiQ0O0mvaj54YsL%2BPXnLU4rifitGHnDn4b2rbgse9ljQXgGu%2FCk9sYvXpWT%2BcDWLtKz3UlJEn3cZcvO%2FfnNckkP8DUbKLD%2FACNyRxww8aHwyAY6pgHqh2qK99L9cpEZvwzO2z7xuq%2FUAjQpPoOOs60SbpyzlAlcbimo73QAV81Z8kZ2oCK7wNVgPNgIOMv3BrDH0YxEZTcUbW3LUhPUqjQqqdsnv6SKjSix8y8hxxZYLWWOlUlI%2BfJPjl2chJ8pY%2BAo4CIx%2BjZeva88oycd7Q%2FiEhA23uKgh3Zdceae658Zzqb%2FvPyssjh9zVt54u5kY51OvOFeoO%2BMwbfC&X-Amz-Signature=e70c8102f950b591ec5421daa384898173fe5dc4227e069d2f5edf46e1d55fd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

