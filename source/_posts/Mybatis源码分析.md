---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM7FVJI3%2F20251126%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251126T140048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEL7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEhSOJWSW%2BWMvDtpQrusplmv1F0NFFygmad1zrnGlWbfAiA8nWP5n115MHRnjiwkTjUXizngKVxFMfATUvzGO3dVdCqIBAiH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMYux0JB0F%2BsJk%2BZSiKtwDkQK53byeklAjFf4GCSJ3xk5fXKP2vviEP01Ork86Ula3uydfcc%2FbrFNvXdjP4bbcPXHKdi5x4Rw6R8rfykNVv3Wv4%2Fl2vvyEa7vfUOOXpyWFQ2YjSYPn8nzdauptIVTpJXhEoHk%2F0Vibs8gUZ1tyA%2BcqTpwXUX2Qu%2FpvMNbLqIh7Y2OU89jgbkmH8BVV4ux%2BFYyjtatp%2BFW3pOcmG901EeTPiHUrGr1nCaFlP%2Fd7lXk15fskAAwd2zBeIS1abaBwZGOCxcdyXthYEZaMwOGvq%2FGLfGfkYXHsHKZYxUg7krgRnHquiyuU9cBY7U0q5T0Ko3v1k8%2FOVD9rv5rbhxX%2BDltfpaiD5ezhUW9TqcAvv2OOBvVmKdXrG7YSep8Zem8ZIv53mWFaNGoV75G09WhH1qPzEgZrvfhHcmoilKbbz26ifMHhHvIkSghYH06gvAUfBgauq35Ol3kH4K8S8jhBdrYgoSf7XfIRN4AWzPJSVV6wfGhch3GqkXAWxuv6ZSG06GgGuKkW4XEHo5WCTpkjJmJ1rtMyuL%2F0Bbew5q%2B4Ep5I7M13FXiEEVCkTICWF3uZhJQd6XWV%2FnaF2HR1pbLtiYk%2Bq%2FmspezgF%2B3wbteoTXOwThkL%2BhnN%2BEYYggAwy4qcyQY6pgEK%2B6peOfXlzbJ0cV55tLTC2%2FbbBgB4C8kTqEz%2Fx9XxKKlIr0qfSOZ5%2FQERzou2iwUp%2F%2B27FjiRiaf%2BgVJ5UznqiJjyLqS4PhEtVEcEsuUNJSOvXziP2O23O1oZ4cXFJWCcfpu2TzzoGlglBRr8znf2k9z9O37%2F7jTBqV378LS%2BFHZV%2FZlgQuVdzitz2DPw2zMzM0ZVGI3zShqw4qeHUuQZkhhTyF41&X-Amz-Signature=6014aeca3662ef2ccab83484583e9567ba29ab5f4be6c894c7a1a73e56b7ed7d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

