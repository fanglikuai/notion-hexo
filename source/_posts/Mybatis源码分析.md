---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YYHSHUMQ%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T150053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBoaCXVzLXdlc3QtMiJIMEYCIQCITCdOkvOlDKnQ1riifNL4sG3B3%2Fgr1ilD4kyPQV50IQIhALoGYNJeXjZHS%2FweaaJRWTjO9cy%2Bn2YyYcAML18oNqKsKogECKP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy%2FtGLW1R06ZbnD%2BYcq3ANr5ABLfWNgLtekHVBCgKMmFKwMHA4pD2kEggwdMm6OQbJ%2FgwBez2b6I54jzA11tu3%2BlRuDpVfCxeK2ExZPX%2BMomBKuPjp6tEi1vrdfa1pIcxsx0zKROW30YVck3hI3il7vpbrx%2FElnMCQhDc%2FtXuhK%2BIZavLW3%2FU6q48wB7dARjKbo9GbycJSAK8eX9aF6djWV0Gi31dbWtP7%2F6TszhQNS%2Fi5WfC6In5%2B5h5gOGVNbkOZIl8xzUCZUtd7ArGOElrHq8gofxBHPd%2FmV5gt%2FQkwCMhfDHrli6I9DeLsZgvUOMVzx4W%2FY1McCpqlUIWBuhAM4EQ1lCeM6qixiErp7cbBqYsjea6DwsmeTIgJFdKoSj6FvRhrSuWeWw4S5q0unqyp%2FLvMb0OXZp9UKi2YOEpsyISX2LPl%2FjFyvnhLY5xp%2FqNncKk1Sk66VVnQEIGo2Fe7bePg1fibpW%2BUNqE4AryZwOhi6hzwS87ZUBeB7lWKvZaZz2gfybtAZ%2F%2Bv51RmEUZBg1iFMPYFg6kqjpxLf2w1Ko8vbJ4g3rgZePcCkecZXbxxiqzgiaeYXlAGyiqlEZ5KPsUgdw%2Fr1msEB%2FJH8pUrcJ62hF%2FldcQEA6f9nksrR2uqRYtzefGKYy2XB0jDT4t7GBjqkAZ841ma4rGE0c%2BrGXxUsqgI0wUQ%2FUDrcXtT57JnHM1rSOTVX77tPLmaBQjKAm50VfuKSJH9ACsBnK%2F3nkgYDGJjHGFtYFdnsQ0wIcLwiSLygi1SEvzPivUrtdhKUCsDXC48TogwiAc0%2FF6VWOrAmHDqmGz134xyi7O0zTPO6Lh%2FGil1TPOV%2BDH%2FkgzRFIOMLe7Cti%2Brwxl8%2FHZZGJR5nM9eYWLb4&X-Amz-Signature=c75cc29fa89181d4f73ea97d1d039b9a5fccf9dec15ca66a258446413c492b63&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

