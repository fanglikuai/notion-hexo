---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46632V5CTJC%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T180038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDc63EWCU005y9Dtw6iQNozxUc6B5pi0wCIDRoJYdoHwQIhANP%2F%2BuiSH2OA9f%2BvujEvP5dp%2FbcAgRIiZVnKN%2Fh4qQpYKogECKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwByJGWdc8Kk8be7Ucq3AO3GIR2qWX3BXHRaeAo17Xrp2UPY9riN%2FC4y7g3TTANOKjgRIIjyIHLziPNFd%2FuGxSj2eH0IJglMTiIhsMr7%2FmUbL5OqHBiZ4CO8i2OYPDU0p2HYMy7%2FIWTPeEONREenChZ4lj8e3VFOY3MnmB8XyEGk%2Bdc8bZoG7D9DVtEVr0nrFpEyT3J0FbvpcA9EtFFbq7woeH8wY1ERUB%2BZvJzIJ%2BOqDBf%2FjiWettw5dO53B3XFgCBBbX92BmJntVDOXPD53zEROelVzIeIZ7ygN3m7R%2FcjrsnYxkpsMOT0KlEKNpQ2kEToewY%2BZmwcgDq9MVJgus0UpCBv9z2TxgLU5pWElllJihOmQ%2Bv5BFsR937cV0wnnHUG2VPmW9ElfimiW1EzEhM9u0zA5uJvrUvmMWn4RG3tur%2F6jjOMom59fDo%2FldaOLY3TzNnuLlfgj0%2FBZg%2F1ky8h6xfJ1DRzCYHlSuU6TC27UNG3sHlTsaDAt113Zy%2FHRRLeRpBmyZeJXE%2FeZJD%2BGlVAsfhwGJ7fYem%2BE9%2Fvx1rQrNYrx2XRNVTMg2zEZwFae75j22%2FTQq0Pp0OTLYjDNpm2oikGmbX72bMTASsa1hljMkebJoevqSo6uTFUJowYDJfV5N4rEXET3sisjDOzf7HBjqkAZXhYQ33H5%2BHKmrl%2Filnmh4O%2BFK7t1GXxODNs3QVJWaKfOTK6hZDrbw9PMDIFIjOPnSaUSGRq1b81SBwQL34qGfXReSC3CueDaBxJVR0bhZa54qlSrTULC9bI%2FUs7bvg8roMeQGP7V6zfXdfp19KqjOb4Wz%2BWK9XXruCsfwOF99f0la3yjHbm8DRqRKW8%2Fqw8IJY%2BmvBDL4%2F2H%2FWG0jgFQRzuARW&X-Amz-Signature=d1b14345061eaf0b580d4f5ccb39710f5b556dec3b053f67578c1111c4131087&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

