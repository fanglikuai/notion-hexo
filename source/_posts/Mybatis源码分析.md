---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ADBN5RS%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T100041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIQDtU%2BDg70k9h1vveDG5UJTDdLHM4e%2BocudJTrk3vNF8%2BwIgMSVk9v4%2FHFCtBkK2Lgww3%2BOIxaDTJGIdat%2BUnZmJbEUqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLuDUU8fQizuuc5XcyrcA58PlmUgBPvhhxNAu%2BfH%2BHmN3LYRowbHXkEDRmQy%2B%2FZDPPUJ3RMweg1QkyfxjtsQsbMKGnEV8prPL8b%2F%2FmaB4l%2FWk0dCiwoZxMxnLnBH%2F%2FMH3QU8Up1%2F7X2nXr5WZJaeC1gcgfAqLnH4f3AVYVlcFWiZCzZGgAN6OcfAbsgQ8IvfvR7QVMwTa%2BC6BTB5T9bYGhQX5RAJnsQSKP%2BA89E2ISzuKY52UNPFJzRuvpV8ntP5rTQzGT0%2B54DrkzadVo6z5RF89HtGaMh7V7VwgxEaebaQoTSRYzlfPtZ1NU9qdfwCrnMxty41Ge44oh%2FVofmhKhmv71LHV79bn%2BmHR6cGz4iE9DL2Xgi9RVtRTrQB2TZBVxz5H2xynpOMYZAgaZB6T31ieyfVTMzao6VWe3jKgV9c1SwHRcCDBZ7bwxly9etmGtESZQrf3go4uhACAyEy3O9zql6mZkI%2BH4ElcC6lQZN2vrXvb2PCwrKCnuCs9M%2BShYB0aMRTaTwvwZjmTCNbkQcrfLh%2BHFUtN6I%2BY4U0iFjAY%2BCEXojdCWAkpfNBKKibmprsRC55C2hnySaw84ocIl8D%2Fl6M3GK3m0ZhTi8YZayHYBCGNfEoarKeRirohnoiKib4xQsYlANhGO79MPrip8cGOqUBexf091CX1ocIk5erp%2BBAo%2Bg6dK5XTF%2BxGzbinZA%2Fxzof%2BxfKz%2BtU6g8libOXS%2FJymrP%2F6cKkWRv2kL5a63Q7Id62ntnWltYsfnY55lU4%2FF9UcJ1KHtC3L0fwnGUx8eTchT9chCdrOOA%2BAyKsSKJOcNd4Zcfd2w43iv5HSRtjwOsoUMD1y2hy06PDNy7q8oyASpA8bMH493SDErJ0vYkZ8QqJ0KlE&X-Amz-Signature=30c345a7c66acdd8860c119a2554d45b6d90638ce9fd9c84b2fc263fc983f1e3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

