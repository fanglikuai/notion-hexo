---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LCDEETV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T170057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBv6w6n%2BR%2B8RImPW6K6KCtf9Mg%2BnzNOTt%2Fvw6r2gJe4%2FAiAPX32FADjYBx2Q76QGfpe43zlpvPqMAMC3gr%2FC3akqjyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIM7kHYbxCqpDxXThU2KtwDHv6ueFlc8m57AdAtA%2FNoWj%2FWOpy%2FRLPTkr%2BwIZEonqHXOhAlWr4Yq4%2FeN5LcKermvAWYhMibV4%2BtzY0nV5%2FlSuM5OyfUxmD29mIPO5p2Jri6wSRK9HyN7kpzyQZConkkk8BhqFL6DtLkFA3%2BRvOEBx%2Bng8yMhtDLBN%2F0VM2fiJJUiIKJ7ewwWtm4agWd81RDjjTCw3XyYNPN0G4q2xXzK%2B87KECd7dauX9O5Mf2FgPbk0eYGzXN61EmSZLmDCrZDHNzJrOH%2F0yqOblpYzGwRjEpzDkUlRXt3u5W2h6NW%2BUn00jjLmsxOXpKlC%2BR0hGzVlH7FswJGdlQePLM6MUtwtvMXh6pi0U%2FetIMVo0MhtrJaUb9SoEs2mUTir39a7VogcxTm%2FoigNmfoebH1dQ4ddCEzwY7QNoeOSjlz1v3TTmO0Gc7CTnCbCls87wQtVDmOhF6GqJ27ifj40w0JeevW0eUdGYGzIlhRWQcf%2Ff8k5HcZEjpCjd5TgewlJsBGUM6jGzIn9PZ8FZiwD51qtrGEx1Al5oedL%2BN5z6xiM4jbgD3Ap9VdiEwIlnGv9AJFUYHq8bbNrtKKrAoOFOagaUB69iaeuOltzgsYiSkltAtQlCXS%2Bj0w7hLCGn4A%2Bt0w4pzpxwY6pgHqT95uxPdlKvQV%2BX8xYea7g%2BLIkwViSTyyNeLuyrYx3LBbg9iut1VTGnM6p4HIsupuVq1pvGQmQUJoSbG9a%2F%2FVgi6IKrcaT2SsmT941BSt8FdqgZpS6K1F0w24qFhr7xJQ12yXm%2FV1MxafBZhARIunFdtBV5%2BOP036Sbts5Y5xmCrggBKFvHtLPPZscVcy4O98gZTJXYVjil%2B36iAveYZieNmha%2FFH&X-Amz-Signature=42031d830aad8822503654e390e81b70f3785795fe608526069186e0b13ba2b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

