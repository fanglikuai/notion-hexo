---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XNRONTI4%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJIMEYCIQCzjB8a9umrMest%2FwX%2BXWWU4hEJg2OAUaCXQ9Fd2hBo5wIhAIUyWgZ7VoeOraP111y%2Fhvar4mzinctxTM4k4dKmVjjcKogECNL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwRM3tmIMm99PQouOIq3AOM58IyY0x%2Be8JdC%2BA3u7yIBtei3uGe8tjAHSEdbLzKUFb0mpY4YpaWWOS6U%2FFPWqatRT9aSR2GfvBu4dnf5XWs0yDs7a1OaKCuyfav9QGwpNgXfz8%2FbKNVvVjNl70%2BdUvHrmDJQ%2BDHlKxzhUdPDrq%2BWbDyF8rkootOnUgHnHrBFBstAdAi%2F7QQrmUv5fy86y4WCNO%2FOw0bS9s3rUJ2geJvRWiEzNCA4gLuAF7EymBzFjeyhzCLvakM0sKBg4aF0FoEEdqM3QB6TfLynartSoQLtnjdhnt0ts%2FM67%2FMPZMXnIhEMsh0nAmO%2BrVWQSAR1%2BWW9F9iBRpH3ZO4b5dJo26LzU0hDmssWYzXsXvPJI9Ggfx12znwpLiQnTC9QrVSEnnyvuuzNQ8VbJ%2FRsmMju3l1cpwkewVkc94b%2BIXnqvZ%2FZnHejGaIdlknIpgRjfZt3HM%2BSaGwHDD8%2Fx7yll%2BP2ZeQL%2B8JDQ3s%2FxTTST7Eg2leuxFb2CZiJzKlmWDC9ZlXQq5r99%2F85AVlAJ1DVRg%2FSCK%2Be6SkuE%2F9lsd9SZz4TFRdYS%2BHnhvDmDehXzOGMhANDQtkfhmGengi6komB%2Be6y8YddDsB1UA1NkiZS7eajiTi8VB4LmYIv6tAHnJYkDCJt%2FTIBjqkAQbvJQaXiKn6LH6aRHZ8qco%2Fsy5UiQDUkD6CJM78Qaie5jyquWKnff9E9%2Bxzuocj%2FqaKT2dp6obZ0VHPQMiRpSWLn9xxyD33Un9fFOIiqzdsP1NDlV85cMxcAVDZi0zC5otqu1j%2FGEcY%2BM4OSZiqirKKzt3BP43PWpxCdueTrjXQmt4Xfz5WUJFLopcer0sTiMeJG94VqtjQl8X67Me4OhsQpYyb&X-Amz-Signature=11536806cca1aa111d56fc7fe878e0cb79664cae2328771d2dd7ce59d96ab330&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

