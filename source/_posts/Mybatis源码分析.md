---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SCUYUZWO%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T220039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICic%2FwWBrsqbRavqkyUuUsCLAaRolr4FBuAeflOjoBXVAiASw3kZPcrX91C2tVTbNyhOa9eSoXTWVrC84XBLdRTuhyr%2FAwhmEAAaDDYzNzQyMzE4MzgwNSIMv%2FSzRIdpiWxaVEePKtwDSl4TkrHcYUZXZbByMNh96tB0k%2BzpjnH7vdBn13gXvvzBtSkR7SGHD4hYUbFj8oi6QgwLQOjibiBWYlf45lX1j7xAEdHdeBu8fPi3QFStA2dVlbGcZEhPtCQcFQ4OpmqT3EQ%2B7NurhqBuH%2BuhUKZIDN1l0VKr8R82kpajnE%2BGvmFytsdv17LIfzm7C8QaklN0mKHdNNpmvJL8tP%2Bm577rycOeQcr8dDNblmQwkI6vzjAQ3aztBR3rEEm8Kf16yWw9Xc4oXNsze%2F7e3nwWQh7ecAQDoE5ocODqxuf7Llq5RaQcHkE1To9a1QSwKxOAdyJLzvwXYe0aqlzeq5GhIuIWgQFIYCK1%2Fi02%2BZvCTmCdpDftPtX5iKxvBnLXxqmUHFQ2pv41RYeUZCTAM%2FK5Zb2cSOJplxctP2MvAPoOE1lYdX3gBmY%2FyA9LUNsmHiWHl0qpxQyplwnVFaGCqmTSgJhJqeS3m9F4YxpGRUWqw2NCeZs4HuWXKDJSRmDeLnKjoeDwDiA3c7%2F5k89tX%2F5UNfauBknv%2FfVRWC3j%2F6e%2FjXcgIAydCKm8Nb68q6x3YBAvoC%2Fdrx21ZB8Onupl%2BhDTnms%2BTZdKYrBNJ4DdgbBCO5jqYv54YbWukyjMnu2Zo30w28LRxgY6pgFxyNYZ6QOiIQu4uC1hw86wam9dUXOXx%2Fl0Zhtr8uqpglBV6Yi1FmAZG%2Ft7ekJ5k12lvspxSdlL0WFjpZqBGq2BCIzH5qy7unzPD%2FZawHjkIt6qRgdOaZEM2nFn4r%2Bholq2FsDFWRujjI8sryQct5n3WLYwnmpYBkS%2BZqzfMole5CEWRuu8dwZTFZDWHptvudUh72tRTPFthzVgtoddWj6PL8nTabnA&X-Amz-Signature=f600df6c0efad18d3d77d4b726981e85bc8a7bff20a6e3411a29367e8605fac9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

