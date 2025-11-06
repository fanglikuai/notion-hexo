---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ROD4GWCR%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T160041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAl2rZo%2FjstxRd%2F83UhB7Bk5g0VnaDQJtQuac37OINDoAiEAyLCJxXUSYj3Hi9uR8LtibyGNpkMWY%2FB3rPLLBTY7fq4qiAQIqf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAEDktmlAQ8zb%2FPxWSrcAw7882NXtBta74ZAhz7An%2BrHxIDtvg2AqEqLBJFDicnePLZIEjv4PIvVKdUfEfciSptN4kxOW4%2FeKwJpJ92GpkWam6rXySdFAieeTwwJRjMt2Hv%2FoiCZci0UUg1FZeTBmsWvfyRTZ0ANXDgdcoqSK1iBWzu166zwo8JkuQDomc9EhMf8jPHejHiyyvifWR4QqiIqhZ5iS0vmay6%2FIbKHMEJdOTSat%2BeBFqyEfXDXuvlUb0nkqwgQuxIhF4uQKQAyaYe3F7I04lu%2FyH%2FL3H9fyH2YX%2B2meBLIdsi2Ek9jYQ9nhvYru8ejPsU4%2BLbWlgo6TO3UO8sX2TFT2906qPNaSvJtdCqSRx%2BrRd88e8x8Cfcy8zHRh1BOzkfpLVQP6EwlqbRW2fKcKB%2F%2BDiHGNBSbJg%2BTE1g%2Bzs%2B9jP3zXr0BSnsbYNfDXIecoEraEEopcAo3%2BZaoiXs0jMJVfv2RshNV81HAyYF66nBu9VUCZIdX5BJl4506RlcDa%2FVL0KUBEhvrMZtg%2BCVUv979sCz3cvWzhZLKugxhSw3bT%2Bon0wXeZ%2Bm4SFHgmA4%2B0gfb3HKxiR%2FnB6NK3Q1VlHfyQAoD2pIXgpzU3UqH%2BzqNGJn06m0WNEGH4ro3zZyLU12TmEiSMJCDs8gGOqUBx5%2FNvV65KTqKvVLGgIeZjOYDLGZNBVYXCVrku3JGl5nmWrjz4MLIzG8I0kQUKB%2FQs0fBqejOp4kR%2Bpg23YFcylkoZP6yYMnITlfy5bWu5rrw3uzl1hH3DQx%2B3WdYZKbtAXncXdSQUGu8DmNa2Vk2bEyA7POKAImUWZXxb4ksG4CqVZIqSbocwZBCrpTwBe33zCVMSGTh84JFMJ6qhakbv%2BaiFE68&X-Amz-Signature=7bb5a5162288ddd8b6393a1f7b072b357cc8172428a571c928f5c720834e7c21&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

