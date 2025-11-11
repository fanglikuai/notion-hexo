---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YMHWRFFD%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEF0aCXVzLXdlc3QtMiJHMEUCICJe1lfU%2F2MCYSLLgF0mIwN%2Fn%2FaPKG0EYZwdDqPFTDaJAiEA5Pi4Sg37uCB1e1lRGnwWdtrboafq7pJcTwHANgwb4Zsq%2FwMIJhAAGgw2Mzc0MjMxODM4MDUiDI16lTsP25%2F5ukOe%2ByrcA2gOY2lJLqNQcnD7L2D%2FXMon522ZuXRZ03gOW9asWMOJlqLTTsex9XtU0uzdeZiiTqj9y2Nkltnvht14q2wPe7UYLzqFMqK5DSCHJwo%2FASZw%2Fp%2B4gVlAQ09bOkbmPM%2F7EQnO40Xkw8%2FmqCez6r4izY%2B7NAr%2FkcX3Y2o1w%2FdiVSll7TvX3Fk8nyGy3M4mYQ29wY3N1GhIQIfKWrw%2BRSPOVGdFbnlX2u6kXHV9BeNLVOJn0FHpls8ZpzI1zDTclbSAwiFICsvo%2FCX0G0SCEWY1KNAs05KGjvXNxilDDhENAQXrKuaik8g722%2B4LZLUUrjLZ%2B%2BHjjzaS5TO9uEIvyuJNUapy1Hyx60F%2BM9oCEtoFKOZMMG8SA9vjhmGlqbXPJpdeTjFsdgi5gTbf8jcIeJVG8tAb2AMk7XKH0APON0u7VBMgenNzls33WwvAOG5JjDgcNh5YUavnnd4ZJIcfkNbxggpGLKwKhr5uiIFWvAaG4Gyb%2FaQyaXiQI2hfUEvfB5eY1yHAaAIKhYptsfrs8DJ9vnmg2xpyeW%2FSU2l%2BMJKZEWabT9AbfQ7U9ldpH2FG9pZlt%2BLtKhyhtUUKUzo7PqpGLGQnF%2FqTEWkSegvQfW%2FyPlU31g9DwQP2Igv1jQHMKHIzsgGOqUBGFxhHAKh3We7tSB9YSYhGuF2Ghtll59Z1%2BjNhgtc%2FGcp4j29JQneIxD3Y4B4HQ8jQ4NYnjbuNdYsXXLXXVSI9z%2B2kLgNARMQzVaxNNfWBJikyGbaG%2FESbURYmdkZZU26%2BVBTDr9zSPhpr6EaLZ8lLuQNmdh4TojlS2ff8gm%2B6XA6KVSiT8s1VObmRGV4kWR3uhXK3FXa56pG%2BkwTa5OPr%2F82hEnz&X-Amz-Signature=67e4dcf0c307332d21b4b9c1c128ccc9dccc2281853e15aae3c5f7bb35e40558&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

