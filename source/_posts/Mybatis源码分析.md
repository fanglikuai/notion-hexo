---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46664OQOGEE%2F20251101%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251101T190037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIQDEkghuyXG9QlHgyUGl4KPdZqDsCwC8YrfydbU5xtAlhgIgZl2hHrrKhXyR3LtiGmcD8D%2BSMnDYUoqHMkYb4neRdScq%2FwMIMhAAGgw2Mzc0MjMxODM4MDUiDItgywClHzsh051LNyrcA0Lm1mDkaIn7BF6TjcKS8k8zofgkEqYsmETdM%2FoxI9By1FkBNbJGEjAt43lB6OgbArB5yiXg6847KtbN7Cn9b6iGQ5pdwRlBqFgxZ9pFdiic7Ab1E72%2Fxuch8uOQBeUS7ZOmqjBrrUzgU0NP5B6D6VEJZQTa6qdUUMZDJ%2Bp9mHxFjLl10A93GHZ18ViVs98FII9v92uzHIlmq9LlAtZlCBch4ixUvSqcxAVivzQbnwBWcwfI1TlMyo8fpNkVzfAlPYpJYjDWoZ5Gfffv6HNpXao%2FYEW6sG2qOvuV06T4JoYKDK9u3HNVU8Ejbs1zMILR9ayhee3ApFa2FXMwxkUDxHfmKQiL33bA7utEZaAW459Zx45Mi8L6HKDsMrqMfhtpKeiy8Me5rcENBurZ8NH4i1Td8vlHc5rk3bfwS4BxNBbF9snJvKjpvYTk4BmPE2YMX7axlIxfig8ajN%2BDTT%2FLdXtO3HIy%2F8wSVnMw4ftWdS76Pcd6yqlKQK7BC2pKHwPjlB2sSAuBWBfvJtpl%2FNk6WzcpsxjJ1i3ytKj75XLN7yxUSTDzR8eXox%2FPMkeSpeSJ877Azlill46itZbGv1Nu3N%2FeAWNWCUR4NbwLMxQj1aYYvR5rTZ2n9cQVWj3kMPf4mMgGOqUBtUiQUuvX0GD2GvkfH%2F0dKruOoQ%2FXJEarCAoCRnUKx7xhBvPCcQvZAznr7Ti7LWzXG9C8pecuums0Rv2m5mT0Swf5QaP2m3hA4SNii%2FG1ByWNT65FKBJ3QdxWOJddXn%2Fo65tnmx8cfBud4X%2FeoYh%2BVK0vMOe1g31rlpNnFMy72Z64ZTFJfkVsxAOywQSaGZkN2sOP34j5pEMipT1x6jy87XnTyRS5&X-Amz-Signature=3a34dc81fb656be11b0b6a779fe64c352d61d193ec0642a9df011352a060257e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

