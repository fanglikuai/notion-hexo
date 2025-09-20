---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YUU5UFGX%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T110041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJIMEYCIQDBTZ5a5YjHxPc5CALr6lDoK2BpbOHN3%2BwhUfwXxDdkvAIhAPeX7GRkNYlej2f%2B6UkxSisOkUe6sT6gwTi6Efn6T6TYKogECOv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwoBp9a34nvTqkIbNMq3APdUBeaexrHilB%2FMNxiB2w%2BRUWorS2zdH%2F3ImxATOXMbiEgGabXVE1c1cL%2FCpT3lr%2Fd04JNQU2e4JoRtmmcejeKBQNLjgqt%2Bc1CjpKm4%2B0qltW28Ej7pOuQxOO1hlzayjp0g3Hze%2FeQ0WWdMD2sQJdLIdKj%2BW5uH0Ni1eOtITs7XSetkcwm4AgZpGSycRyBpBRmWASbEI3swNwJd4M%2FD4YTU%2Fuud%2BL9ErAu5guD0s8yLkTk%2FN7yBHpMFrbnKnQoRjz6jvOCe%2FjdjrxKqO%2Fwt3eqra4Bxxh9cShdYN7gbKrtkWTqFYxIjnypO7d5FzIgKKUIDahsZPrhR4TybVxSCnWnJxscIifCLok3MLLKwMr7OQvCe%2BKzdaLWjx0%2BrBj6rm5rrZvvvG3zJmRjEmnMCQEQOaXMg288w0C%2Bx%2BeYyMLOuaMmObejRUtiQkB%2B5jRT%2FQMJ%2FYXacseYjvrD0wapGEUizhjn6gAmEt6lXPV9EJgu5qJu26kuMHMCD0NyQEpyCwUPKBROHohSsfPPePoSLMCYrW5SojYPW7Cfyk1HTIfEMPUqm6Q8Hw7CBExI2066fHN4g3eZk26QP3FfaTKu0CgDfhQlIwz4K%2F2vE4RK%2FuICVdRk3faY9Duc4OPuRDCv6rnGBjqkAbsMETn1apvkCWd1c7hSHofM86fZAkvjQjvC3%2BqVv653kjQXIMYRbIaseGmrKgcEsxnP1r1lScVwK1sISqTqn1flCrQOOsTG1NGREDmNAs4U5n5pQTaoJbGskR%2FwlQx%2B4Tm9u%2BEbXQAAEyk9bGlW7%2Fq%2BBr%2FsB7foCwOTakNrHLuuLbN%2FDTmzhoXGDZ2s%2FKifn4tFaVgmpAG3kQv7JI7xSeWabQAO&X-Amz-Signature=fea4e0609037cb18dfb700ddb783e335141d12834ea909601bf4da86de75195f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

