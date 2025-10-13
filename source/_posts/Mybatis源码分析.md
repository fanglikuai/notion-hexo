---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIMJ5EAX%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCPtAdSWAGO1RF9jBf3J8L2sJkbnNXJYfx3mWx1UWdxQQIhAJlBTlTIBH2XzGyOD4piP9pDvWmAWIP0JrC3Y7eB32w6Kv8DCDgQABoMNjM3NDIzMTgzODA1IgwbHAV9y0YGenWKM2cq3APxoUrWwFTGyy2g3%2F8947pajp3b9Sn91tTou85neM5tRe7m2VqgcURTw8nGk1a8J2HgN4%2Bb5g5mu83QAPym2FVhnSHrehJnupiS5c33HBN3P55IK22L1hSMaioj%2FL148%2FJNX6myZB0AIHJI8b4malx4vU45xeGX4n%2FOrXNq4RAEmtiD3IfNu5hV19pA7PKIQ1iA%2FQru1e96TapUbBsWJ5JewwF5wyURj3c87SwhGmlJ6Yc7J8%2BzcSn%2FTDOGE1lSpsGFNVH9GwQL6WhcibOEuoGIilUGxCfL3weuORq0Af%2BSiHW1ZguUYV21CjpgPFySlzjHhfDDpsWzm%2Be0LUVDHgMc2lkR8xJr%2FC2tGuxpSoSEfzbV7SR7aymrKpHgIB%2FzqHk%2FjlyxTNbdZ63Bv5A1ufj96STJGXHKdgE3O4Yr07sy92P%2FyEhfNnoXbvgXrPLLUwKla2pPt%2BpG3hWIT2i97l2N%2Fm%2FScsaNNO7YDiGqpDe%2Fecs5%2BjnJLQSicoS99SQxQdJGfXuu70U8C%2BKsqdTA%2FvVbr610ualwml%2Bj6SL%2BJSTKoW7ocP3V6F5jE8uLEYsw4bzUDabD5Kl96%2FMAcmxHEQIDD9fxTzXNalp5AMHBpkCfOdQSc0YxavoktLiEyDCC6rDHBjqkAUyDsKW2C%2B0YMMwaOlwJEuxQI3QVkU47KwyO0kkoJ7FHybAr4urZjN1wSky3WZdzFBXpUtToHhk8WgeDqEUQLmrAV%2Fm7riSbUzTy%2BIr27JS9xZcqq5u8fAepalR0EcpDVI9ccdksFf8TOdZ1%2BINC4zL7fZ3H7n5OokcZDiDWuzYSjzdV9HpggAiVLu9sEfGGjX12PO%2Bb9Te82A875awuSbtRvF1P&X-Amz-Signature=82b70f5679c40037f129b307fe85e54e12c8c0b1e3c57ccd3d4289bd5415ddd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

