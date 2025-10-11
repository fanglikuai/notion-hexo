---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TRTG3VC5%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T110047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIAt46lGXiAxPQelh3SFhKUgrWPio3r7adtaGEM7vaXYOAiEAzNoo97Z2gnGjzmVUHBKTqlQB9ufR9ffaw6bMsadJ4EEqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDvwFavEPxwdtIiqlyrcAx6eApopVfvPa6YuBGEbxfJ5KPX2nMFUCo2vOQQkJIpecNEBmKGNbTpE%2BPpmRJhdlB7wypDLhctwwHzir3HsO3KW3zx4K86bjzp29132t%2Ff4Lh5VNr7%2F1%2BXW8wnaLgbqSTYGdpJHxV7M5PFjNj2jipEK5LDJGbKx8RQPyytWI2OcIYRJ98%2BnZhOKmTM9BcFNRpEH2%2BKzfC09XwwzkA2UqzjNGaRiYNGxqEemWN3GB2tp0JsMubjGBkj0aZhAtHvZ4yP9SL%2BKnXrPN4snfm37SvxYnnpp%2BwNil6ZmHsu1SH8wzQ6LFnaIWKfar%2BevYBMSY9s0binpJ5HjFPouAxUY6YC1%2Bovbl489NmAItNntQapyJFyaC7A9%2FuAyFFfBone0pCjz8QuBPV6yx2ZiE1xvToLSzLgKcWIfKmn5Rw2bky4%2BlNMOsFK8NEKDwVJVnk0pO95IhXylmm0LNwG08julaprZl7W%2FZIkByjxw5n2nKhH%2FtIhwp3O4M%2BWJvW6Cpmk2xINLXYK36GXXq%2BRef7pAhIROVM4s8f4Uzzt%2BAh556HGIxzmt4wLUcQrjHs32Ax8X1HlI8ZYiK5yax8xINyyj0bsrWVQgM6s%2FOu%2BSK7H9Z5MblC8enKR2%2B%2FRYCzRiMKvip8cGOqUBy%2FIe69wcU0CPOF7xPNCyHEycu5JzI2LZHfQtHikBFlxtHqX%2BzkWPpYJT7WKNSpYuG2GLTgHW2%2BHDKF3kQGtFnTwlOZOY9JNwDtNKf%2BDUgZvlVt3cXz187IPd%2F2FvW0vLAEVGg%2BQpFdZMj7WqwbrKBm9WXsPv81xA4xl%2F%2BDGpjE7sD2sR1C1oKmfVbI9hGyrzuDviLqtBT%2FKUZiylpEUWC6Cuwk8G&X-Amz-Signature=d910f79b2187c9c2bbaadacf1c5c6aadf82dc6f6c0e9f8bdc931800da278cee7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

