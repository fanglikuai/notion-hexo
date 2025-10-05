---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMR6FOCF%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T060049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIHIx8VstJkzaerLDckYXD99nlumRdtFkZeQlElFMeIm5AiANJKd%2FBoAyoV3y3bE0Zn3paPXoBqEhACiEKYnmLWP36Cr%2FAwhvEAAaDDYzNzQyMzE4MzgwNSIMCq0w488CLdbQn68HKtwDSSEbbV9smNkhkDmrIMwgtudmZ3hESm9H7CIbOAQzI9NVuEqXOVBEGlnPi6MgK3JugiqGt4yrKFlP5uUn%2BITCr9lNNYKlLHURqCpHo8%2BrXU%2Bzi7FwH4qyL8%2B3A9K2ydfD3xfppdU2c7K9%2Fv45%2FlMt3bZ3M3DhLFohCRHSsm9Yvca4gn%2BQk61e2byybNADZ61AIt%2BEdY0yUsgUQUFb8XYq%2Fm%2F9GwGkOjwN4BCgRq3j6H7bd29QKrbrE9jyM%2BY9yaPyhK6qlAfvuRyStsfvdLBObvUQDYEYp1%2FS3iAQnuX4%2FZcxTje95bdCcbi%2BD0L7pdGHYsGcovWBXXRl80rkmRUhZrGS9IcRKfvEk5PzOu1ffSFUBsdN%2BKtiCD0kASfR8lCpR%2FGOqK0k4III89Sfp4csL139Fdl82O%2F7Hyh0FnfERzDw3BPCtmsgKGXr7drkucueXQgKUrYaFZrpEokrLSEPczpa4VmDQNC9pmdr%2FF2zcG67cgwzZTDa4t%2B5OKLVlM1bpPtGZj0PbpmwU8YI0tePm5AqdXpzOxKAnHwc%2FSGfIQPSvq4wywjrrMRktZ%2FrLa6Q5zCA3QDQwJG2bSKlZuI19DaDw1GKuOKubNLY7%2FwDSCPNvcO7n8FZX%2FxpPjEwqYSIxwY6pgGlplkixAVc4UUTiMXlpfusQehhWEekMOjWNzbQXuUFd0Km2aNirCTXtzWCE4zyYo4o3FHnlm4ROr1H%2BokPlwkt1%2BV2w8qJQcb8qh%2BJlQGQnpSQYjeiXlfZN6HoFPoy4ebI7%2Fyq0ssN92yr9H13irUpuUfCT%2BLNgncn%2Ba%2BSjk2U6rag5wm40t5pzLMKbiOufZQdettrmgQgsIq6WP%2FyBGefDxEE0XA4&X-Amz-Signature=67c8011bf7bda3dafc20d6a05a9b4ec4c3031ac56ae3f86dc2fcd809d0e06e0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

