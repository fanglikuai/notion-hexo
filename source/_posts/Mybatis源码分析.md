---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SLLL4NGC%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T200040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJIMEYCIQCSMnS7ELCinKn7uSujjF0vsXWsBPX3ZPswbFOKETQFRQIhAL06DHsUEShARR%2BOOWosw60KYCKaDRcIC8yftIgyak4JKogECMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igzyz%2FP2OmUGhr43zeUq3ANL4RpHvq98Xre%2F%2BW21gzDgyH2iR8JO%2BCz%2BNDwUkIHndWDRRgyeq%2BVtgadcNldZPSkFLWW8rDY31JRCJoc%2BRNO3syyiP7GAUCq4xjLvydh1HN8UO0%2BXytLZV6VkIWpbQM0%2B1cb1jB75vJ2%2FsgeYoUefcCQWpef%2BTqFK6h7HURexgQeW6K6%2BsScS0amoChYna1CHuVDMwZEdtq%2FEl%2FN0dh1EDmBdje8fb%2B9y5JmNxn%2BvcCOkyCw4T0ZvqDPLthJY3%2Bch%2B2iVcmsn0zVMySL74oJxzqm1hWkmerbtQNxCPu3r3BnB65jLzmvWMiCbhMpKC9Oro67en%2FsfNI2lHPlV0BINP0kV3KR1kpLoq7zm1pSc0M6fw1EkOhigz5PvlnJSFkWBmo%2Fk9la8KqmgWWRvz%2FumBIiZ2XQ%2FIf6xbR%2FSE13EJKVZ00i%2FiXdJawySyiGk5vBVBQfL39jw%2BvtJ7ArcMOqzYVAud3v4whFT0hX1tBQCvrtKpYI%2BqtVyI8LFnwj5dJhezm38r9b98IwPtBPz%2FkekL9V31r1Icr6w931jumPlzyNZjdJtuu13I7337Z9y%2FU5iIFL%2BuTjN%2FLd91Dygyh9xg8xtSmP0a6F4gDNJ9oShOVNxVzHYT2CjdPfy4TCmiPPIBjqkAfgb%2FEZEdqOsh7VqKhCoZNe295lT0tr9WlpIsHAm0b2u%2B66K35WFIf7rDW6YOkhjsCY0Iadq%2BKU0eMGq%2FYzro4khz58zdBoBGB98nehBzz1HbcNon2O5R3kku3pEN5a0yMvMaBVBn6Akj9cG8%2Bze9JybIJBgWYE%2FwbYb5O%2FAxye0fk%2BEl0p%2F85kKQOThp%2F0of1nG91loAX8sagbfviPXnKI6JZBF&X-Amz-Signature=782b59dfb0a2e6d6936cd8b44b19a0b8da5b92dc57677eca26635ccb9232dceb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

