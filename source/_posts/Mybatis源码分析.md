---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YW7BXXP4%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T120040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0xXVN3YYG2ibkJW9bwo3pURfh7zPJmPn1SSI9gFkhiQIhAOpX2PbK%2B0h8%2BfcBEnZAaIVywzyhBMq8K49Ty7jTPu9fKv8DCC0QABoMNjM3NDIzMTgzODA1IgyQmUr7zNKC%2FLp8rqsq3AMKLpvAdlPIVZpiD40jBZg4bXMncpsbjTaaCnPPox7O%2FmzGEruYcGG8y57Xm1W5eeCFlSOhfclfWYsyM6GDqvLO1RkYihKBH7jykVzhcAT1GzOZULttNJxic%2FXu9vWFhWKOtnJTMusR3aRnVID%2FluUuma%2Fo8R4kbfdhOxkfjdpD%2BmwtVpbr3rtyq84FawBCkJkcXrscS07b5SvJ%2BYAfn%2FU3LjjRH8QTXMnz5%2B1ck9oPRpn8TEdWF7rPg96VnT2thQ1n9zxTff3lMr0DIYtsTHJ4KZX9dUydHNG%2BvxRI7z%2FFEo51IVTyUt4hS5YDvUKG6%2BFGn7TjSlB6Oj5jScIP%2FInA3wrJswTP7hVtPJKnkJWl8Ifw%2BTX1U2p0zOmbO553K6gpYmQMWVp49O%2FUoPGs3OJUAjLhGbRUBoB79Nsyt24CyTWIdXvBEdVLmtf5LBHkJBbsNAV%2BAAyxadJ4AOBDH8QImg0evlXSAaJoE5IPnunrlnwgoUhIWUCe%2BrPWB58yzNgth54wkfb11fKJMccnhoduJl2sxLVuhj9CUYKDlhfz6mGh0M7hbMjrV5I3SatKo0uSaQmeSV%2BvYQ3lwb9HsSqS2QL6FJjXV%2FL66RpErXeM45S9YQVbMX6%2FXN8KXDCo7sTGBjqkAWnR5RU5lIfxd8JKCeiBDSO58WitAdV2oZ%2BTfnSGUs5YvNkZeyo%2Bd7XIPLENoUap9W3dTbggST4xhuKRYpAJjBUN8VHB63JOsfUEkIwe4MAdD3QIgYnuiWqJLgp8FteEkFz8PLV4z43FOP7%2FcrvBK1otNdtiw3TEblup4%2FEb0YERKbkY9rpG8i8Luh2pKHQi7CA7JCtsuZmivm%2Fu38wIQ7w1jK4l&X-Amz-Signature=0a1238bf00af5a07993b1c54f50e24968ffe2623289dcdc6cfb18941fe2eecf9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

