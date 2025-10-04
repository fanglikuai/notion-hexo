---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WNZWFQTG%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T070039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCmEaGOq2Adg0ZUjovQopSGlcUNmxQuM2nwP5MdYXoIfwIgA5xR6SbzO40bLRvqmsa0XQ8PaZmWrSeXJMtJzU3tKv8q%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDDTGVzAOL%2B913B9bOyrcAzvXMC9EF%2BwxaqCbJiWANyQ6CPqJ9LMc0x5n4zb9uEMSnC3ip21gnwkkRjG96SEY%2BfLTqu3cWk1Kp4080Vpxnwr2Wn9Nc%2FcIbwTe2e37s6lLTRvg7IjsSXTTzrTVyZOGwmgUDU8NDvzPqvYZ7mcAB996AStqDknkp11q0UE%2FG%2FukJxXEsCaDtqdyxI16GkSPW9TsAMWWH4bcqAEbBf0s%2FmfGueBOkQKiKYAJ6rQrsld5ey8baKhx3x9Zzi%2BivX%2FE8nebLhYgk7yYiCnJXGdO6zu%2BEMf%2FnJaJ%2FQREXPOYCoLq70UO2eeZT5J0LF%2BEuXZpu64zOo36oWUZZmbBULNi1hNmPZSTOVLlSXHUfDxfIOKe7NIVknzdn574fJOj4CGWewvrBrIdE%2FgEKZbwgXVFEbyx3KDT2HwdL5S0pLLNAsWUIbapKmUS1IHtnkvn6jybFZwHOYgRhEkbYaATIcC0zEYUDEq99uf1et1JsZzaY7%2FVaZLUx5nqdNrStfcywrsG2%2FKh%2F3Fu8xYUKt81DWQn2auBsAE%2FZWHuE5pRlVYmaLX3bLkPJSzjnc2DVLUU8QSe%2B55sMc5y%2F8TW4f5Za3K8HG%2BBZQ8CxvxcR0s13IFjILok78OT0%2F6SiSSdNZLYMIf8gscGOqUBoTHSZkyPfvVAVWTJbzWnQcf%2BgzF9EZFuJS0UH%2FnnDVtw3czTx%2Bu%2B%2B%2FF22wuMWAc99DQIld68pUi46TeRrt8MURUlP8q4JnfCd1fSRtKzcjnaojM4e5mPZ9PBvVebL2ifKL8CzkOy3CtyQpDxaDKySjLcn6yZATAI98FoEhDCWcApgf%2FHGQaM9QAozjaxrn0sWkeWm4LfinM5NSgIecsDxAzRuKh%2F&X-Amz-Signature=1004cad50cf48f1aa62dc3235687260bcb32dbeec3825fa712cec248f4af8fa1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

