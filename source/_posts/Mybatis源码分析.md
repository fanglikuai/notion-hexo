---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q6OTXMAJ%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T190043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICadG%2F608L3KbG18FDEkYam4Mo8k%2Fq8Hbrj1btAj1CCvAiAoU51IlpNaN00y5lZ%2BLDFxif5iUn30Hs8bS7xAwrU1RCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMPZ9kV0GCKVAhyOKCKtwD4v9dFCcwS%2FbDVRyQ%2B%2FJKlhl7T3em9khMACSk8gYRvTqpJAXJC245SnFgrXYB%2B8gmIACkyJ6hnOVvuOmoT7jjdMXkTh5dJIO87rC1vPtKpmSqd3tk%2BDjyB6x4FULARvsDGPV5CagRnx4Vsu4qwTqIzscYl08Xd9mm7BtpFIqDiCiMUg%2Ft%2Fcgs52GRe%2FOfEZwqQ2HVGez4SSb7z8Z1jYhJpFZWKV%2BsK8ZbMP0hjYG42Shj3zh5cp%2F5fBDwA0qRK%2FVKfonXccUerG%2BbU48x6ihedGyykEyw4Do010gO0zOUR8u9a%2FlzEhtvoAz%2B2QgttLIG1G3NsBCBpJqwC0ZZ9qSA57UWJbvZBJD1H1lqj9yB4um8um%2B5PHES0inxnpGn%2F3pTcQOuobLq0GzzLRZ3%2FfLUlUz01rXjy9%2F6Wx9ItDojI%2Fm9XwiEckwaHiMwLDYdlkUyIMS43kbrXm5hsEcBiWmzCFgqG%2B%2ByvIwOCMHlt5nnitPrEOy87c%2F9B67QJF5ajMBUXYJsIbRQA7M9%2FaB2MDtQnGOE6sYnFQ35evZtBMOl91V5ptEL9gf50Izo3HAxPmp7RhmpDX9vqNElAfATuMGiX%2FOfsOgT1mRWuqzzQT6rmcsVG3yMpYdxbrPyyF0w58XtyAY6pgF2beD5eI3gOeL10N8ALzC7evqaCi4eNfxiKyjqz6qyELbBCxbnfrUPBK45qKRmees5kj33L%2F2ZSZ4euQSRR86ONANQNtzibm%2BBzRodDShN5SJYu%2FZoBTIkbVmtz2%2BY4BOICBW2q0KCYEud4uPXxN8LS3Jv2e2S3hWBtO8jROorevseP9rtNXu9kl7uSgQ0naJnkR0%2FM5IavYIXBODK9QLj%2FRPOh3TY&X-Amz-Signature=0b9a03934fe180e95001f32eb27c484934afa576e40b4be0a54a23a8c93b4b82&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

