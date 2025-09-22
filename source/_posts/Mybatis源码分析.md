---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662MUZQGHQ%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T110038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDBjgCp84ZQ4AkqnHFAZdw2QBCdbkazOPuLtg2uIOzMQwIhAMk3Qb8aFoh3jVsQ4NPlluNB7HTjJqynxhvB2m8FM16LKv8DCCwQABoMNjM3NDIzMTgzODA1Igxnyi8TpGvqwznrWdUq3AMYwD6O673v1Z9112R59JXzMtAsp%2BahtoM6GQvQBgLynEgRnOkMgFtGRLFv2pWCFsTkHuX4hh1Z9ScS8yD8YFE1ve%2F1Fe3esMBMMmvbydwjwT7%2BuRVyi%2BDc3s3Ou1Iw3KLZzeX4m8QV2toEBhyFNQm8K1aUeBLprJeUnUIZYZjBW1U3I8MW6icn%2FRKYTFHfB1JXaSobCn%2BeJuguRj8aUcVsJIl1xD%2F3r4nKWDL41o5KEBnQBxhjbT9dSLYZgiDwoON8JgUOz6KcheDMHNwcBGu0tDZzEQr%2Fg31moRW%2FR1pv5MucMPdHVaEZmg2KS9c7e0jBCL8OJ5SaFgahs7fMJ4IiOv0H9VTTS1AnxilXBan%2BMLLZ%2FzEC%2BSTh5Pa1ex57Ck9RD1twKXrYcB%2FF4tI31%2FF7Uky6U6k4S1QTDvXCKyNHy4QdgXRX3K6517TSjUfSo741kFMEk%2BC1TkihDsma7hyZ2py7ThyMqVFwu5Ego%2FbaLddiYoxntrYh351Y134%2FCfnDiGCZ51sWda3NhZo%2BKWoQu5ek6bsrMZceHL8O%2F7fYe6Lct76CoCTUu%2F%2BAbxkfTNuk2fByJfFyqTTpA4qvXa7dAs800Rag3ZMlk9IAvmm%2F5L3LNItvHUXqO79v4zCu0sTGBjqkAa5Xx%2BAgsFj0NI7CR8nED9FYtKg%2Fv92MYDP9H%2BB2Xb61sMTFz94t0f9wVbm3nzzhmx%2Fibp9cksyOQL3EiaUWy0U51Aenmzl48JsvQS5nMFcF0WbPPmCcC%2F3kzi7zUUzElmc%2F5YkTbzicV54HNxs0RAdw6WxLuwd36qNKYav1QEdQP2%2FYhk07SA%2Fqbimd82XKXQ9oofBE6emPXnyO8cfjn3k6tj20&X-Amz-Signature=f8f937d9076e0c0e448a3d31e4a271d710251392c800affd67cfe5d2fee305fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

