---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VCRTND6C%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T230041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJGMEQCIFDHCUQPqQOtLdwfIVdxU0kyA1rXS0p9L8EQhX%2FLKcJPAiBWb6taLUVrbdoJQeX2eqKztQ4KITLEy8hgs0GExdcHJir%2FAwgXEAAaDDYzNzQyMzE4MzgwNSIM8s2YuJ517uoaJis7KtwD1eRb0I7irvgORz1e1uXQdNSL4cCEEur1RFh243fHmai081ZOISlekoaIZ9WiKpDsEJ1pWZNt73g%2By7wTowbK1cDabCg4RXUBLHk45wcmSLl3iXxjVZMc9ljBN1fKLRj0H3gw743fA4aZKDUCEt5p7f8KoCGKsdYkm4hehm0rwQSr4C3dRiaeZ9%2BgrXZpG8sSb2DNTq2UZj3so7aZ1jSEBKSp79OfdqhAHjX4%2FWUh9keQwS3BmFu85Vx8hJbvaEXWL5jTbB2yuiJ3eAMRgkqzsfnIhTAVO%2BJOL%2FqL2wHtXtns94PPkMAc1zVmJFgumET2iY55BndsoWvxBsEaIz5JhgTMKNIlimk%2BYky9ZXML88VpV3if4gkARjooN%2FqPQdYQnFBUjMH9YYxWao29KzFCqpnIR8DzMUMzTJxRDUamTPd0FKuUbKtqvnumhRb9T46pk1%2F%2F4wFQHRLZCE4pRIuxpkBhXpwNwgM1PZxNj2fTJVf4Oa27yTGkEEu03ZLCR51fR8pBwas3sdGBPXo56k5xsiJK1mzvLDjo1fEAKei60U5QsRxym8Gz2tTfF77DgFJSbYYmw2LY0BRqsadmA9RYkfFKzI%2BUU8lXnJtyjwHphIuZwEd6kjK6oKX5Fsgw1MuDyQY6pgGcnpCoTWlL30cv2QfOf1VjsmpeJKXLGkhtQRpbztSZDknsd9BheRitqAXX1i7WELu9TI6XTEJGKzBHa5Y4kryd2cZNlaBT8HvHvbHmpMICAwX8AAdy7ocE3GGVrFxqXO6w62ebI2BB9EvsPM%2FKh6Gm%2BWu%2BVz1zcpjtt46ZCloQUDrpmZSbo8%2ByP3N6x19UnovHGy3cGaz2iRgxkNeAUYk9cxEpp30e&X-Amz-Signature=865f679efeb6b0c49be438d83d04cccaccaa0c785d28fb31cd16e09f3766f86a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

