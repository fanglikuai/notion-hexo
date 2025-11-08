---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TZLKEHNP%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBEaCXVzLXdlc3QtMiJGMEQCIEqeLDzorVmaXEyIWhDbWPcnRUDl9zlAXQ5hsky9rqi6AiAUPvfrjrSOAj%2FJ7KctaOkFIw0Fo0gaH6oUM2RKH%2Fa7XSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMXVkGRrT5R%2Bygr4NUKtwDO%2BoRoqv7TcV8R2Y8f4LZI6yOMdjpVkPJ4Lp3atREszBc01ee4wX9%2B5oKC1VGqVSdmufN0b2TaGYCveWFizjkztTsVftP%2BYbLKWxfnypAtdAvbcOl5934e5x%2FzxshSaK03NxyCVRjz4EsY8S7Ve7uVUKCU3yU7X26RthFkkMY4FWJRQ7ST%2FEaOSpYBUG6S1RhxezB9Ro2R64PNdBkFlqy0ZrAn6t1vZ01jWtPKpekh6Koxeq8cN8jOW%2B8gdXjsZZJhZzoAUnvqU1uSVo5erW1Ts7aHp9X8wffHcmLDUm5yb2xZEImbt5OMh1%2FSaNnR3rk5M8MMhHZrVzgC%2Fl6DrbrcTacGOToJ1WYzmW12lVCz2wJiWYSRu99Zb%2BkJQNxIBV3ug%2BsU8b0NIPWdHTdGK%2Bxk2MIOEnCm%2Bj3ytedk69zJV0lmkZ2aPGv4iejrQx8cRxvXiwVUtanOpwXyn%2B6erRhk82wSabIKmF2nsRpjxiFpGC2d5He3Bk9aH0c%2Fya6vuC%2BSDctwjZKc26zCbaFvUnxgmjXmrvl%2BdVolR5kfDx2aYC%2Fdl7DqgWRyPhIJFm3DGPgCi4XWDd0uPBGefTz90sc4Ls65BJC2BXGBbbLa0QP3MiKL08jI3c6UDkkNOsw7d%2B9yAY6pgHtG%2BKwzthI3pd8i9oBm3mCIosyUqbhXc1DXm0cpbZhC18B3AJ73AF8mqTVk6HjsGpHeAug4nb5W2VzbiIAvvT7vysMYPkExBvByhsSmJDltKT3X5TLkoHVMQUF8tOK8ydtdU%2FSQvRcgo2zY0CC93KY4SBan%2Fk4CHx6fxBNJcLQML9FbhKT581iM2u7vxFklb84cCNncD95Li%2BBXJaC7UulFi4ixuL8&X-Amz-Signature=608edb46a22cbdfb465342ec120b95f1a8eb69a394d0e2cfa05c37ee4f499665&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

