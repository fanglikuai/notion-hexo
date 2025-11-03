---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TE4U4CMQ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDvQ6ka6yvl105BOnldrbRX88coK0Kr6NJZznoEMrcWcAIhAIuzfPkg1H0QH17F44I4t9G5dW%2F26kyfr5mNJeI6G0xGKv8DCGQQABoMNjM3NDIzMTgzODA1IgwgF9H43GUIkBy4ba4q3AMxQaUlnE2WFM6wTs7HykZYR7W6%2B%2BC7TtiiTB70SjqmESQozqDMZ8kw1JpXAsGayVlO68mtwyVGR6muZciiVvSXOH9ArhbIWhg3dU%2FyXjZvmSZPmsk%2BAmgUHaU9oEsg%2Bs1QC4lRNMQixzISP4XHv2%2BJSRSR%2FSHoF%2BP6gtmDjnwPnkhTqKsKs%2F49c%2FrqFVDnz2WR2sTT6JGvYpDx02Ig2Q%2BHtzMY9hic14NwPYsPhY1T%2BVyeUMzng9S30bp8g%2B%2FBmFHg54uQPNilGYxi9Ti38BOfGVl3e2mhoWUchLj333vgGFzX%2FbqkVSEZM%2F3QrQuUPPSW89%2FL0TqH7jH4deZNKGFWQNmu27QF0mPN3rQnpNaLyVNmAF1sl5W%2FICR6XjwFIgFhBdDm%2BB254LLuw%2FETszXRwoEKTHe0oSYkR2Ieim8VaL%2F%2FnXcWZAQmAU5FPxD16shjG2VqqJbPJKsFicOKRv%2Bq2dJxF9cLJnLNF0en8WPGhgUhG78TyxIGQcNOIC5hf8DHF83T7csBzeOiy5HFRM8fZws1q%2FSR90N5vaOd3jJIe2grYrxGagEkC6BSu1XvWBJwB%2BmfOT7gilCjFZpCqAcecfmvHZWSt3Gepa1HPP%2BnMMANhrskcJkNJaJDLDDt86PIBjqkAXA7vm2OY0ds5OMgcwWsbNgdBIXQinx0IylCvbGc3HHv65OFT4EEXnPohT7c9ubx630zjjk89fjn2%2BLI%2BpcVq7%2FKzHGzfVi66cn3tSd66F2eGErbrsWOiufSFFchSx0x8moAxWoJJH9mONIWkZd%2Bli1NghtjhH9PbsFc1h7oZOjVeqb13s6B2Wsl0A1xToVOzr66YJgEhyG3dX5bNK0eJRbwBuN0&X-Amz-Signature=6d0141696b3f50a022fb443db8343f30231809b5a819549889a197c4d7577e5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

