---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QY7PHIA6%2F20251102%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251102T050042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHQaCXVzLXdlc3QtMiJHMEUCICG5XT%2FMlnrxgMwXFpaz4JVsS%2FBWgtJxPM4fyNLXOFRfAiEAiYbuN2xxSU6Ej%2FoPofCq%2FOHfjxxbEVsY3Hq6H0TBHeoq%2FwMIPRAAGgw2Mzc0MjMxODM4MDUiDPFBHITbV%2BHyts6M1ircA%2Bwis6T1QcR2iv2qKTRfnBjKh5MJord%2B6aNSdl62G2pzIlEKXNb4YYCnCb0gHPZ5GsOQsptcReXw1FfJboPVuMJJFuxKCcQansCAShVu5VcRT%2F8xDrQlw932nkzJICsgM2VluBa6XSH2ETqcp8kvu7Tj6kgZJX9QhD%2Fwgr2CUED8MXCwicfhalNbCst55IG%2FREPpvnerVA3l1xpK6pD20su4VUZnd2si2zPL6pjrgQZRcW0tq8Ej7olb1E%2BEOS3u5MjBUFwrbmHBrBI2utzZqN7iKCStT1yRDFa6Q3FNsMVt3cYXojLp%2F1OBaG8JFpzK%2BEYwhQ7g1Vn9UuQMWoEQKhBOIAAbet00zOMHgJIr9fW7uunRlKqOUgGayYVJ%2FmTZ7AhsZ%2BHNJLvFpo9bmEZab5SpbPBKnP2AcLndw4b9W2CAXS%2FzUeQ2PX6bSpRGhE0vTV7x5U6qUlXbDwc3Cv7sFmZN%2Fqs%2FnxJPsZee8dnxKoO1DIIGm1Q945o4nYBETaVygEiRaWnDQMRRLFrh4YraSK0CGWy6%2Bpa8665JbR1ZpwbtEX5jaaV5HG1C3Q3vC1xpuDX%2FjqZ9Ry4QSFPckl76isHhKf%2BEJi3PZkzL2D%2BPVwxwSYtHAdLV7DfTk2yeMIS0m8gGOqUBKxQGfJvHWibIlOc6s1MLOi3%2Bkv3VGlqa%2F9XhnmG4quUj6LFt28OnkPyeSe3Rzw5uXvU%2BjL36dk1DnG4XTkv%2F6HAQgYJrLj67aZ5NBoPTIrCffI32rcTA1xOk8LQ%2F3cTlpiYbZXyp2zYZSQMGptefMuUXDVWqLH9kuuWcytbJcl8uCUeGJA3ZMJXjNelbhIiXOgLtmrnf5SI%2FmsTCcmiMIdzrcAO5&X-Amz-Signature=79d866fc485894791a3ecafbe817a7d8a3a93d78c346e5de0da1e8730f70c6e1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

