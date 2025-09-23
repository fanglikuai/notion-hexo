---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZAGIKH5A%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T110051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCWKyhPT7uZwhA1lmcNvROsRV67lIrn3MRS%2FxeexA%2BJXQIhAK4KAWeQ%2FTirh8ujDrk9%2B9ov5p1DXTR2tW4l0kW6igqtKv8DCEQQABoMNjM3NDIzMTgzODA1IgzifE3mOHeMl61FQgoq3APFZZEd4Dvu6YPE0wx760StUedQSltT63Lf1cOBZ0InrYwNcHoIpwWvtpL%2BWhqY0sS%2BQ5zx2VyIbcOXfLKq0UOu1x2UV83iDFR4t3qN%2FWxnjWd0vw8bM8z2E8UrK6i6ffZZgHOT4QmxEPzjX6iC7ayEqTjPjZ5xRcQp4nquQnmKZXBE9JQSAJzgG22%2BS%2BursftR97X0fApb20iDoDImB94KSiazzZvgikKbF1e9c1TritldCHJf3seK0Ebtf%2Bs25QAqh%2B9npEctrCSbJ%2Bpzs6cOIeydVCK3PLQQ2xbKDJma0Z0KozexvVjhMMlDETCGu8lqfVqh6Si3G3V4o%2F2yrlzY9n4ali47O%2BNygvrOqmNBPEx1Yna76dFv1r74myG7wfjTLZbM4yKZc3CE9D3B4Igyedh4d9A8%2FRGdDASno50apIZtDexFUblyLcUC6%2FswmQ0RhFCTi9KTSJ33MsHKOZykjX2ee0TMLINo3YOcm9Z4g5xmHPoAQdrIcFAQYDDzqBb980ryUPIESe4%2F6UJKQ2Itr6rqv7MccAqkg7NjpZn5xGvHWRZHG5Grpfgz81p8sxq3uiJut%2B%2FEpRVw2pirZjHvItRXV8awlNq56zJqG8bLmStAtUzFwRxHlhJ1TjDG9snGBjqkAWeAQcCzOmbtbUNw6uIQSJLLhZeFNLoctFVfYaMrvcaDXO28EZQY92OpuvYiU7v%2FJRkZi2zx4nJZFY%2B1fAQZb%2BEGEHdpPtQmwI7Pf%2BbO1L3VQVM%2FGe6IE2%2BbJPOHXmUBdKJbZ%2F%2FbxKg55Nly3Cd6LaFb5PHs8DknY8RG6J6I%2BiMAehdMZEWKqNQNPeyWYlgwJSu68Utuoz0Z19IpUIzEqRrtl69Z&X-Amz-Signature=e17948e5963590001e171ec2e5a25140079074d3a586b8445cabf7f86faa85c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

