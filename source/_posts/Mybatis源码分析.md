---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466545WSYR4%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T110042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCTT%2FpG0u5j8RtOXUAkxQxxk5raiLF5rsZGapnMrfgc%2FQIhAJYBc8YVH6p%2B5rcwxTs7%2FrAECZyAjlx8HEWUnaB%2B0C4XKogECIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgziLzkXX95f8Gayal0q3APxcg7ohm7OhYm9PFGOudPo%2F4K5ucyFs45opBIFWGK2sR9a9d%2FYHdRFnhlhZYCSrSaMsyxtBL16GiF8RRXx41KZbdD4oopwToQbdqqF%2BeLnzo2r0VXY%2BdtND5PwLWsEBSuZBVDc9%2FNRKRRp1uDt5rXmGRe7t3bVzKHdXwc6JYcSw0W4F571kh7642%2BL1Z0jaF5YRXC49RUizihDJ%2BY6Wn5GmoiIwb9FlwRpLUxuBB57SqGG0t%2BRqvSMwi4Eu65I8%2Fa3Y%2Bmg0ntckmeWXwHRASlgNb9Dw0nID%2BMAH6ZOlVjc9%2FxpEUy8AuFvfJn7fpeLfH3%2F%2BCN90g4Bj7N7eWYH2GoMbZgXCHu6bs16FYw32fy8lT%2FycT8beTmna%2B%2FtF%2BLAmbztsQr7y7Tq3cW3ERltUhXE9TMpkL4G622NzDxzeR7MyOo4JLiWxxZVoD%2FxeEAYXE9TWkmUHMuV0eHfO0yfCSAiGPLSl2zfKKXFnn6WP0uulnWLJ2Q4%2Be%2FU%2BwlEMRNGZ6tkmDIelKKKfX4Vyv5EbxVbWqwe%2FHknmWMYMPnwo9KmzcRiHjeDqwqIZTlhB4Alngge%2Bb9njUcZI%2Fvi50XSYCiHrQLrtmtqxXIpA9JQBxMHVEKiOJJ%2BoQcH8Y3AjDDZjpvJBjqkASQpiyfK%2BaFd2gMfgGgIRWyCKAU77zkI0aR%2FCxcwmuLDssy6Jxba1aa%2Fll9KCvAahtNgepokkJHmz38CycL9ifxpzAJ8DtMIh732uLPGIOZx4d4T%2FcivAEbqvR6pajP%2BWA5L1s1rcBy8XusVCtwjfIOULHDMPkbnVyqnjl6TqqOnp0Abr%2FFuUh5oS0iZsYbMG3TI8R%2BU3VLAV15I%2FSrtDXbcQgex&X-Amz-Signature=ce872c955f6de5dcd0d7f317567af2040621923dcd95ef142dc00550b98a7843&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

