---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZP5RGLCK%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T180045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGZ1bwyAFfgAYm1mFNXhtHGC7ZPIr3XOZFip870f7ZYDAiEAnaTJyt%2B9q60Lj458ZqiGKsgvuz%2B6ZvTe%2FZUFi1jOE2Mq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDK8E6Wt0mSJc2F0V0SrcAzqca5gyOCVccC2k8ga3heMse5Zue6Z1xKFnZ5As6xvTiO9xoYHKVQN0JWkTCxepZ%2FLvLeld%2Ff9w%2BcuCj%2FSlIKsZZOP3nA78bmjsZ7EO4BFUHZmgnYgsdiGTdohNksP6mmZMVQtEdwYX2tjtNOGF%2BjCpdHeAyleoTgp9t13iEoFll0OQNTW9vCN2JK2nVSgiB3P4nw1mz6I1OkEnXD3SEsKwX7yJ%2B8gosiUvDaoLnre8TlHKePMq9wUafEVbdUbf0zO7Bp8YFeDGPx5ne5Ch1WVhTsIE5qpFYd6nfG1D21w%2BEv%2Fm8tLf67fAYl3nwtHwpHH69ATc7%2B7CJ407tkYnMap1ztiMIUvLY%2BTbg1l21CUHG4XA4wG4Amf0RA9TJgWg8Yqk3H5cSMLGUfJsqglpjcprNVnNX5fv6muauMgeoLD7oPDVsRTwYRgGJYXPCrJWYx2nw%2BBJ2BnTUlFRbC5zNsd788lme0E2Nvr72HAJp5GWgXVIFKA3l4%2BZ0ufjODgDaYW0xTfAYBDegxLxu8x1%2BjDhUe7kOTpiokKMgdl2VHFPBCrdGkrpqnwVXv%2Bd0heXD1na1SmQdUk1Xf0xABT0utL%2B157pv7DpNazx79Ei5zSsZP%2BABoJgKNK9jZ6cMKz488cGOqUB4McnxWis3NegkNNn2K2Px9hIK7qmOsTTwkPhJ89jA9cKDCWtnd%2FFuIKSXUrwlFLEra%2B3LYMC6Z6nVBiMgjkZ9jdH6U5RxhQ2tC3miLGT8kdVCicFfp9w4hXGzNk0xpdqK6x3HLX06j9nD8phIDOy3nDDxeEOUaO%2Fhnr6z2MFfrlpyF0Ll%2FGTm05Fza%2B0dl10Kz7JXOcwMLe7Lv%2Fup2d%2Byfvrp4wk&X-Amz-Signature=abf2c995c97c2400ca63c30adaa508da85eae12c347028b51d0970d530f90b96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

