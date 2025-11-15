---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ONPCSGH%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T050054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDfMOE2OKz4RlwVabLr1OP%2F%2BG1vnFkAsd3T9b2RQcdIXwIgEl9VL3SQxcqtZ7Yae1Enzlw%2F3uPUCW2g%2F8aKPg5hZwYq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDAmRxRk3g68qRU2t9CrcA%2BVpMsnlbsdzh6FbVVNuzERkm8nY7hy%2FJ5y937l9VrtTAJGQ9TvuKkGlAj42iRuX4OUFbwyEOb2E%2FafSzzN456e9RnSpYj9w%2BKT%2BgG45t3p3ld37fMxMRJcBp4uakuZ8KleQUQ9RhSgfTilr0PiSyaSEGqT4uVqpFxOpKvgwIo%2FFo4ek2msCHzI3BZFDHKc5yFHVYXQ%2FZmdyvbZ2BRd1PuRjBvO0ntpums2p1aMVPUqDc%2B%2FJ7EbE6H%2Fb%2FhYnJjX53pjrSOm9Q6Mg9c0YYwJ5dDNdcR%2F7%2FxDJ3%2BcCBi0Rf%2BFwsjYHaGETSE61pnAx8BHIJm2jwvMAiQeIJP8ekFNB6jvVD%2Bi0HuH0hnIPqW91XPVS09Gtbc9eVYkkAtpKa%2F6ohu7Yqdu3gP1bQ2vo1ykvWUe5aVjOMrVibhdnD8fTYaAapJAYPfjgSPZEO1bIlXCNdEzCZG6AkJdDirJKtwbnmjNC84oJKlMooMHt%2BLLS8Dzr1s%2F34iW9hUa2AxduRAYv7ivgQ6D9jc0GDUVmf7TrTkaRRUnM4ShiKqPvi%2BI3QrqhQ2DqBmOiIBL%2BcLcl2wK8x7sb2aoRoazjJvNqbJA%2Blz9ff8f1cQms%2BSQVyI%2FhsfQ3d1M3fjJp2QKNqIcQMNOE4MgGOqUBVD1YxENr28LfrMm6VHhhKUO%2Fg0xuXIqRsedbd26qGyOzi77VY%2Fq4qEl2%2BK3ZctcEicj7h7HMZV8xPnQZ8geQ86emdPW8PNXkQBxKcXnOZdZLy5buNDipcSOYX05NOlYOZZBv0CvzcFxFXb9sAPNlKi%2Ba2D1QgBexEPBnwqwnXXSYxijrVtGXD28oEhVAN%2Buh0Vh1OjoJwu8EMgkzf36uAvBd6i%2By&X-Amz-Signature=5ba6d975e41c6f7f024bc3b1ec7f3c685f67ef0ee8dd052255369ab30354cf78&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

