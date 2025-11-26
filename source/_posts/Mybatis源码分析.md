---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46627SMQV3W%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T170041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFurg6QodyscnITOOGB32BHcEd%2FucmiA2V3cG9BeVZ1nAiEAnEYbrcIPJ0fSdoV6nnWc9Ji8AL07w4%2FtObafCe2oanIqiAQIif%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG%2BfEMvC39vxZcybcSrcA5Rr8%2BqoTIdWkqFz%2F%2F4FfpB06N9QVXOj5dGEFFzTdY2vDz%2Bo4UvRxxy8UANc1lpiNh2Y1veB%2FHdIHuqOvXQLX0EeCOmASNQYA3o6fGK6JuYF5LEhyP0GGtFlKTp90mX7KspnjPr4XeS7ciWZ8SaY%2Fa%2BAf82KzgkbZJJUpOZIpEFu8auk7mav7XRJh%2B1WeryV9qtt%2FGs%2F4zrj%2ButeMNHPk6SdBK6dyaBmQicu9r9f1BqcX%2FrPKoi8Ck%2FLZ%2FkdMoN0WfUdd3%2BdrSonLRFX1Fr%2Fegdp2Ga4xQ%2FZ%2BXC3keGP%2BPV5Jb0Jbkh7yaChDzOUaYWMKmQ%2Fwu5coDtFqogzrf6SPK3%2FGJWuU94dGa03AmtvecZClmXABmsKBpmzsCwyZGAGLw6914kcfk4sIht%2BNesIj2Yzf4yesFZ6Tfzv4s2sYjzO3%2FUXah1COye21jYYg5KWKkJs6qKgviasfaPGyvfZ8iO0AdOBv%2FEjSQq1qDfdUfyZXwI7aLTCZVC9CRz9QB25Sik8A3S0sNR2UbZustrrpGFLmbJi%2BpK2iXsR%2BWzRiDFsagOy9Nu1S1JETtsEJcF8YB9Qwn8com2fk3lN1FuXwLi2BJOeSClcH1KnAEhzpnoV4b1l%2BPAwA5ziSvx3MMPGnMkGOqUBa4zUeV55%2FEWRKrEBGPNWzaIQ26bZmiDTwBXAl5eFKfVKId3O5JjDJLnzAugSkGXpzhD%2FN1mb1K58vE9Qu%2Fu7u4657pnVipgG48IbNyn1DWb42Z0WC6zhVB9Kk7O0iH1R5BD8Ip3AHWoXDv3JC3LIjQxEzv1TwX7Se3GOibZp3Kx0Sr4KZk9I3bEpNxcmyxCWQfyBtnqkuJwiaEP80P7zn8SjFXK2&X-Amz-Signature=9cbac491c2501b112d8217f7ff561fd7af36cfabe004f4d1a1b8c752f3b1fe8a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

