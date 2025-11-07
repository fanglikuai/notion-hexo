---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZS5JKU7D%2F20251107%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251107T080101Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCT78bgqLO0YywnyagS1S7xZhQGQvg%2BR1eQcch8vAbnCAIhAI4cxxnXlBdYoz%2FHUknIJHh3ueXzAjsPYF3%2Fp3monT5dKogECLb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy3P8w4CmLcWxBXbXAq3AP9gVMzZj8J0dGmWcgBzpXy4nDR%2FbrH1c0pVYDz3dP4cfesnJ8Ucd%2BaAmPMtMjx0fpwVYKfTp5gtYkwIgfxjI5qeIppS1aNid1yCKTFWsxTUBkQJRSS1FslF6gOGvbZb4z%2FSzgyeaeebQBfDoobqRzYedJT%2FzNGdOLYx1EhjxKk%2BfAsqQ4imrdjE8%2B8QYnvJY1CMfyX9aYIfPdVlq2OzJzPRARX%2B5XfjwGC5B2TofwYjUPC4vsb420wnF%2F%2Biwrnuq%2Bz25wSPXJj0NbvJPl4dR1zZKkjQiZ%2BzGWe8lp1zoL%2FTHZyfUBe3Fsqk%2FC5uUfd69EdXor4bwKq%2FnJkSBdFeFT8cvBPhGqlYSTgX66Cn1GxZMItzJW31vbQikQB1U5ytjgyGgzj1gAFpe%2B3PrCacCTh0iwjq7lqW88M20kosGteiKe2cFCI%2F%2FuMalKJJT4OuatvAAj3TIQfShcwy8I2SayI1VVzSQDml8AjJgGCYsAvdR%2F1pEh9uGznO9tCymC2JyEPOziWBBH4JF6yhQCT5%2Fp8LvgFTFr1kvc3gvlGPtitj9phwhRUWQ%2BPxfmpHGbqmaj1v0LhdjSTfFrAiGJTQmNIGBHpDrWXjdKCuP1I1Klnt19ePG5WKurEpQU3QjC%2B%2B7XIBjqkAbaGMg%2FYU11lgEkW7ADjmZgNTDX7k3WNVK9aY5vEwdpRJI%2By7DUX1gMgvpbacIQ0vkv7g3fdUb8yd2cci9hCm0O6ElkOPHC3AKOhFdV99dfBQnpZyzLKNhT%2FtGHZlHvQnqwrad1YT5Io%2FVztfdS1VjSkTKXRRuixC2ZHthsQrp91w%2BkUTt6OJGHyJXlGuMfZvVi2ew5msigFWUKI0hpHLoe3k3WF&X-Amz-Signature=5a79f5c120bf4065c0210908f475cf46092b353acc96f06a752b90a2d21bb655&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

