---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46624JJVYUJ%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T000046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCXVJiZgZPSrKnZMUpIZqSkfsz8QI7DDAlYEMlYSc%2BF6wIgcZPmJhShoEjr%2FVnLmUQGy0Iknx2aNMQbjuHXcm91C3sq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDJ2oyQwgSQ%2FYnUMpdircA%2Bo%2B3Gccp%2BBPU8Q26FvdmC4T%2BAO6o4jM61sKnD2N07CqbM9ziIDnxRspjCogdxcR1Dtv%2BEZlRzARPKii2iIRZsb8O%2F2z8lwMlN6WprtozkhG212X0CT%2Bok7rkQX2ymYGZvCEJKDavSlCEJAd%2BZJGiqAwkFl%2B6OlDaAOV3QybHVxKzwaOaMPP3Ai7UqBMU63cnYPtcEf7V0L0uu6smw8yeZZVO0uma5k7t2rirU8SXcOKemnampvX8Kvw9S%2FNdAvpz1Rt4d39HXNVk5NzuyOOGQ3dPeQ5rY5TBX1uhRfqme9cMncPzs9kx1X%2BvrOh76vjxVn0QClI%2F%2FG3BHfqnbZQ1tQgJhZVIrn9vy0nkT9P5BFMZWId3Ud%2BxvxX2U%2FKYBJJwyeEM9wKIcVSFm8Ns%2Fy1EMMia2RaGFxsq5RtPRQl49mxfkZ8esBFWVVa9Gnec2YGqOjwFbrFthwTv3rFiNejG7fHIFw4hlLTOaWH9Eegue5U9JpZOlNNgyKNalKC3wM5mLgXzCsyjKBPth7NyXpuiqs9hPJAeslSjDycMQuvSSdzuXE7IwceW3iMEnJtG2Sswb0q9I8NQJDVIN9OegV8fm9d1NNBS4MN6HY7DzPn7rqtT1HakS250xGONQDxMJW7n8gGOqUB5AxnQQqBSeg8DUcYZeaCwSuNjUMWQJn76C7Gs8wItBiPiGx7JJG%2FA2FV%2BidktxG60ylNDLw188e%2FYZ7aais3P72b%2FhoCqSHZFJ5sQ9n1nAsneCjLvQBOC8BceT6YNmKf8WOo4%2BCF2%2B%2BJJzbkdnneEVV1nNo%2FkqFTNDTaUODy2O1uZKzcP%2F6B4ricpvMSsOO0Zey5o6qbe9NedRnYWLdlB3rQC40d&X-Amz-Signature=b13766936a51fa52615e7ca1f00ef5e5444aed1981fc32b9d5cb317cbe3b9e9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

