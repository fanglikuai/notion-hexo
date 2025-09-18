---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3BQJDTR%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T130145Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQCwMkuIVNE%2BJnr%2B8Es6iA1LAR2OL8o4gNsL0KY8thLbogIgDswDcADc9aCRroRl%2F2j1HLAmSEJZX7QD1AV0d5sSG9IqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPoB%2FPIpsbIa2v0f4yrcA05F4jqholXW1XkJ6OT5iNSwIjYGo4GiVGhoD8cc%2F0k%2BY6Nf2yQVSzMXyF94VsIUxa6Lym1%2BwA2Vr5wYEb7lD6zOAjS3Lp291CsxPcQK4Ytwe3wP99k4XENxe4qO189F%2BkSxvmrXNMDgzUXSadKDa%2FSJMW2y00mwbF%2Bb9Ek3es%2BjDMP9SrqLUdplOcwsNp0Tdcszo1Rqd10fTEvg%2BmlgzMH7%2Bm5lQyoiXeOKfHLShlnUdM1n4LlCAd7CDCxhKN4os0SGHH6JW8G7uYLvNpZ3MEhIlGAq4%2F2RRANNX9X3ePVVkae1mPYNtLPGJz75Dgv0y3T3HduHjZ6Kgns4Or31UOnD5yuCfpS%2BmIy9HN3cdaj3CY8dBdKcWRm8WnLrue99g8r4dfZJvq8MTrw4RU%2BRRRezw4np0h6IfOySjdpmBYjTc4vAnSa4jaKhv1RzyztOYNoOMXpq5sCW3YFdPdP8MKUUqcCT2JsRq12XAS%2BAvGYym5OENdW9jSR0KuQyulgcxrlyCZVgIBFljKFNlAVcv4POtOCM3o3EsWt1%2B7kr9bdHAgH%2BGIlpKFoc90nu9hInd1%2FtZ0Ve%2BdD31Gxt5eL7YEobXofFnfOOk5aWceXLe%2BN6ds6k4GJRpggdYZv0MLjar8YGOqUBIjuh6gU5b%2BhFXEysNT580G3d6WwcClKeqEpZBjU%2FSvay%2Bwwq3%2B4oRZ9pjnJGISWjiDtYGOwgALhmdbcFq0N9Nt8G6wOA3C3HdSxLK6aW3uW06DFroNVxzfc5AkSPmltWe%2BXxNJCONZg81S93seN3IuiEuu4Fq7Pf%2Bf%2BdUqV1nQaCaCbX%2BQM06ahVXf1ZaQJqXud1HuLNoxpuSCDQ6yRkQ9%2F%2BOUBv&X-Amz-Signature=fa6813c9cdb555e02227b2bbf6b6963e69eba33517c20c91e8dbcba3c2052778&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

