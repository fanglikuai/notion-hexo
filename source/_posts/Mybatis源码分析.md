---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RSWEFK5%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T020043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGIaCXVzLXdlc3QtMiJIMEYCIQCF6ql50DNyCgEUTt9XBPbBCZXpcpP6molHT2CteCxJcgIhANTbsU767DSqI16cHNj0VavanwUZvCpp0DNklmSdtAyIKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwh2CbnhTGIeqD%2BJSIq3AMR5a3BCs4pYC4GJHWCFADY1iAG%2FgrR6Px4ao%2BRx0ffdBzsu3ljBjF%2FAQspkPsRNGSEf3yEu%2F6nmtyfxqgJPFk4Q3UxjIjcFrKOI8dIjw2pgCgoML7kEiIMXriK9o%2Bd%2B0Pjena8UY0mrp%2FWRFjy0y1n6NM6%2Bj9NTi23jPwcMm7Sgboxac%2F7U66cTP6P%2BDfDu3vxIxXPJTHdotgAwo1y6VaOu8UArBh2vfHsujFenoR%2BLSwPS2OYwT%2BWAG4pd7eR0PCxHvHnQVtG0vE6Z0OapX%2BplzyD5xPejFiaKHDlOfnZV8Cap4YfWiL18oFZ4Phlnj3Dtrp55zAdvgKHpUR0J%2BsTz1ahx6Q7qijJmwa8WaHeEdBXD9eKvkIOBKYkozw70denpwep9k%2FRhB7hjLfGOOBCa%2Fv9TW3KQAl3aaphMun8fQjoDMVGLKq74gYZWhyY0A2CfTkbNcwnsf%2BcRH6mEVgWgGg705CNvOHVklsvQarXWk8hg1fPDKzMZpZlX2XlLLXKT%2FR7fCw20oA8gCMXo1Wdy9ArAPsbzqp6SiNnjW48GNpXarA3Tsywd2ZH9L63gamYOS3Ugw4q36lRHluqmAH66X6%2BN%2FKYj7Xjdro1UXnBEmJptm38RQkBxNsn%2BDCX5KbHBjqkAWnyf0kxFVz81Ll%2FvzwCPo9gd2G%2Bn229ajCHyXK5RV8jH%2F6zqC54I18GRWYQNIdzQqdhUA%2F5pfj8xN%2BbS1PznrsxItUhel4usOUZw80gGXtPA6iA6Ud%2FrfmVEoLiwLib8%2BYMIejHQKW9t0xlavdBMgsPL3FsXt%2F40uQaQ0K1V3hzf7mc2%2Fy7mk0DtuzBiQchEVJhzNBp26jJdoYTvfYe8gl5v%2FFC&X-Amz-Signature=94ca06a3cc9851990f350219cc777246a2eadc8186920ed99ec54e2d78c8154a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

