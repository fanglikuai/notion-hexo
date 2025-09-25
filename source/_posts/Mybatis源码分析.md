---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZYUYVYK%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T030046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2BlVIaLBoQtzeThaYr3JQIHXM0Y0LmgtE9a2FDTQZUvwIhAPfQvS4UcQNYoGd8RhrrNE682vgQemhhO8BZTGQJSopiKv8DCGgQABoMNjM3NDIzMTgzODA1IgyT8KIt%2FtyXGnrSclEq3AMKXCAjEnxatBBMrrWjmcIGh%2BMsH8nZiW1IRtQn%2FSGrNS4nnRe2i2pD3WLg%2BqURRenZl2En2KPycCpbQQ%2F7ebVbTDo%2FSrXl8O0zn34xtvtKSqozx7PlL3KFLKLql3maBt8N%2BXZPkAfCHVT%2BEh8BqHJcNHAYmxiBvVMriBeV6rfHPwG1%2BWn6S2ZwUYskT%2Fl2Jhq8EJNbwLSo7cMMAgUMWiLF%2FT3%2FhYBgAInOuZQq23tgaPJmZGlEPHTjLo6kNY6Dppo3ZMPbyRc%2FaOqtTACRBhggZ5LV61T%2B7rBqYXhJ4xir10CNE0mvmAzuzDw64oK3K1EM3iblugoAPFpuwP3FC9yUoK589QeL6uQp7joQI%2FxzzcDuHLc13b5PyK0nMCrqcPDpf6OuRCrTm9GDH1mXIw%2BxUL3TPxmgUIwK1Kz9U5K4vwiy2tcFXy8nTbxvhRTOrRQf0VBhHlGnX8Ula5ObZgdc4Co0REBCSrV475HXgPzHmREXTkJg60draEz2x3Fq2aIAd9vl8Fx5F6JjuKNzf%2FmLl3RaIQEulwuiv8gUd2AYQSHVZL3KS7IRwBilj3pfuP6seu1zfwLFH%2BbAuuVQqU4wC56YS%2BAn2zWLYK8ClDebQhafTs2d8kkHeuhC3TDG6NHGBjqkAf7M3SSgsmvc7DfP7JD4JLMBUReGx9%2FbrQlMZ14%2FI4r3DzzxQ0X%2BxUoAw6EXRRXU%2FWJfqrGg44ZIGc8S7mqye6nfrZE4x3hkPhuQVRSZuRDfvoeTbi1G%2B2NOEKefMlSa3Apf%2FGRhRvXmHFeUWMSFhwO6hjgtUkcfASl7lEkFjZYECJiBcVi20ATy7HOjWRpQ9XQBGpZ5DSn3IDqNEH4eDWMF7WQ8&X-Amz-Signature=88b6d00b236064152daa2dcf69a5240ed6d6c81bcd84d42e9798ad43dea5e1d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

