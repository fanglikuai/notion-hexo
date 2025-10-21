---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SRX2IQVU%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T170045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGEaCXVzLXdlc3QtMiJIMEYCIQCAqsON28iguTAZwTI62oRJp32oWltzNz6eViKLw994rwIhALMlmlKK8qPlxqPTMhFMlmZK9Ugie2w3mHNPwUwLHGs1Kv8DCBoQABoMNjM3NDIzMTgzODA1IgzgMLgh4b0L9K%2BlpZ0q3ANlIjeu5nuq6Vl3MgD7qfHejnOopQ%2BZy0cY6eHOhY7bqLaU1%2FxbzccJ7nnW%2FVjw%2FTsNAQxC%2BdzLsK3EgH4cMiNBxn5dQwfvp5lRql40mdwTydxwz1WNX2rybsYjPImD46qLZsovDOVZPf1exr41iJ6aQqWLvzi0j4fBBw5as%2B70zUP2EYLtTSAq6hu8FRcKaqy4ti7pf4zixU4udYXyvauuqudy1Mbmvk4LzWGJ4LkVjA%2Bbjhbe6l0epAz6e00IFF4rE7PSJ2i8gunQjO7AUwbYA6R0T7j9pfUp2AmysVAYzGy2liQe2U1xBOvK4JR3faQzxWOiOGgMX6LWBRbblNjqVObUZnoh66GFwvPjGtRASvSeUSj6SOXPCy6x2BGc%2F4SOza320zTsNI4Ixx%2FTz4c%2FieVn7b2skbCZIM7%2FX8NqHpAJI%2FV2r%2BBAE9LdxTBQpwHhUXCfJ1AmpF8QohpuE73Gqu%2FsBvFObZ2yxceba8dpjUYr80N%2BpFTKT4oZMaTfstyXUbWWnrY5SOeuFS9pjJiWSC5Dp9Uv19y30oL3lMkrsjllUFM%2Bkk4x4Zq9Wm3B%2FfHnJCmjjIDbob0NdvToqMTHRLB64Bepe275HFK9CtodvYIQbAOOPMZOmHFP3TDj897HBjqkAS%2B1BspavBnClnJ%2BMq4OwXZ%2FPC5b8Nx5aNSlzoa4WoS9OGDv53QQxVEG6Cggqm5sjRwv21uROrg4hD2qd0semKJZimQeYjnNEKBOUKwAv0yw4OXcMJLABSB6xGTgBaCa8T8iH8tfdQAXlRQspVxWNJSuO%2BjcY3Oy7RIy3D62G3qxoHqoKles1uVcslGZNgjGIVnH8%2BxTaRfDB6UG7wZXdqnu46gT&X-Amz-Signature=5b579e6b7f04f7d4102938a34da6e2d98570edabcfd14476735bbeb5ee280456&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

