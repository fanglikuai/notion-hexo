---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673722DDZ%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE4aCXVzLXdlc3QtMiJHMEUCIQCm0hmBLsu7lqmJW8gSeskxi40GCZ5vznh6suIE%2Fcy%2BMAIgVBfV4UgCIgKGSE%2FEUhrNGFoFV11aSg4fL84PzOnkFtIqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOjNEFRswgCkrbbDDSrcA27b0goi20pBwCcJSRMMkxZ2PA7NHv34lGPuruXLVNbNdWQ%2BPZ%2FC26kNVG%2FITUebFR9Q52hZE1Ovm2F7txwTIeLaK4F8CxGVRWCXFX6JPXMyd3fXF57EoxsIsnzZA7JEgfKhQYglvTeiLNhHE5Rt64vNOUlmFXyHU%2ByxxOw1YTrWfgkmSSXtMYrQi9jUL8oJ1o6W8B3Mc2dE%2FmRkPORivuOdXHAO3xdiJCcfK1%2FAbJGqPkSFtDxE%2F6Pt8jkD6ExBk8ziePWftx6wS0xgQig6i1X33jwgDFhfbJIRWnii1LzFOTLWOuhdF7dkkMW%2FiWwiTfrHApUlQvt6MeO2cXKgOLOe4O%2FLSNk8T4kvvW%2FupJvkDVCZo7vIsNfFWiCVZodOZLRgJjXU3JE9Gq9tYcdv4RUzWleSzUJXncy%2FqjEiOXVR%2F0zEplB2QjjB2WsS8svsr6nOULS3WUUVJeQZ%2BFpIj8XPBXqOCixodfJUHAvwJX6z5HLwiHaRZqBz3GVwFV9L3OyDkDaL7AZnQ8pDHPCPWqpSNZ35PuTBz6611ombBv8XO1tQDMPA6fCd0St2p6qzcdCwU8C8qtW4ggsbzjBYFLuFNfm3mjdkRA1Gtw3xH4H1MzENPzpHv%2BzCcwpeMMy8oscGOqUB94%2FQzNSwRsno9abHODfgB0HbPfVKaawC3wKPkqz8H6EK3TmZk3hU8fMsvwLI70FjqTt%2BDYVbCLuPL25aMVmjsSkWos4hwVuhtBYhpj7RkZ3MtuhAgEob2PsnrXz4OVjry2KyBqaCoEGOmogVWpI%2FfMXEA%2BjK3mS4jUp70ECX4%2FLQx2WusfOBbtIEXBIWvLUbGK%2FU8ZHoX9Ey4JoS9pKNvUnmY2Q1&X-Amz-Signature=06eb3de171514e63b101e41174c9176ca821d6aad6e64200d113a0fad7ce0f48&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

