---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SWL7A6WX%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T010040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC5Ii2LKjYG%2F6nL8ThOHdc6kMS0I0tdmFudZYzIFAHRmgIhALWLe557%2BL1zFRuhIucEHusZuY29FPfh2juVTWTmxGToKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx7c5M2Br%2BeFQJWXfwq3AP5Fc%2B50MugBjEaZIAFFZFIrClAqaAhAmzBw4X6aFxqz3J%2BJoLMfAGPjvTb5Wp%2BMFxipqTcqyudENlAoa4XKiPQCos1X4nCw3FGwShpswtNBsyrJ%2BwWACfC7n46VHTTQSahe27zTGlyZn5uSOxuRDPrUdMApws0bC6hgwjd3kn1tvHcR%2BG%2BROYKa52tsorDvwU2eCko03YWBHRlGIBelS9DD3E09uNxwWbmGmpsPDaalD2vVy2Mdby58LoPEmzmd6LLpy7l5ReCCXQCq22LgpZHrwLlSDvr2OIM%2FrzJbvoSqm8LN1T8efun961Fz%2FQNREbykEysN0aCiE0yyrKZKRcnQZXWZiIIHYXbN2X75%2BtsDrzUk1MVfQnXdMYuDSAvBJK9IyS8j4Qbma5hz%2BcGAtmtajIX%2BvzYgFz%2BBVR1gVVHr22zlfhzTu6V3C424Tp6eJGRbecpvxN3AcQWhqf74I5SdtOaByzX6RUiQBsMkQubHTS0LBHeiIoY%2BzPLgmTyijbPxhqnkhnLxPFYJ%2BsnQNn6YLZoJaN7CdBLkMUmV2hNk1zYtAhp3p6hjN3qFFAFbdX3YLMfB6WfYH4Ek%2FdZ3brv303ssch56Wfb4o%2B3y2JhVU7tI1QVoC%2BDYI5W2TDe4KPJBjqkAdo0WOVca0pl6noyobC5E1SazZNKyMAjgABWdbIR8OYZp4QGlF%2BQ5Mh3TgfZrIV5lUi1nliwRiLnkBhcjLTT3PLpzx2iYlg3Ytglwr%2BNtDUk9yJPO3Fq%2FD9Eaq7KQT5GufXmORwmr9nCPdcIaJKO48xwwXC2IjoYVS1DTyDEMMqWtvZFRzIVaTaFwQgJczmgFqaJOltDQ2%2FmpzG6CiKhhlCXm613&X-Amz-Signature=ef78c1ce0e742d885379dcbe8070dc72e143768b9952ff74618f5f3aa0c86ce2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

