---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTXB4ILE%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T100053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAzs0ll1i1EMOqjqF8H06Ba0NYR%2Bhswp714EbexVNJzwAiBCuqfao88YOXKFTckss4n9uO14OCDm9ZAGJyLMUN3dAyr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMxcNVL7xD6K39mxbmKtwD%2BmCe%2FPVkXGyD4WXLjda1c8F8MFnkCk9rYuUQcAkO50DldmYIDYo6xSn6LG8zmrWcaeSUAgqyo1QF5NiUWkAMbikX5pBumNXOgF92b8yXWKOq3Ikd%2BkIhppk2S5JDErj0MsHDDvqmTc%2BQ0x4uOhDmwttbqaiHhA5MDDA5XsQp3%2BrzK1dwRHqNb%2Fvo%2FaypwIlDT%2FruI9AhAg4w%2Bxw%2Bpaz1KxbzoYmsy2b6nKVa9SJ6NaLUo8YX2FjJGGIHWOzojt3Onb%2BA0yclnG0ZWSHayo7Chv5pTsq5h9BfJGThAdwPVzuLxvW6h825hs%2BIgTTvZU%2BLM3DL5VsckV6luE4%2BFAYQXhHqAN%2FsSnausxIoSryKPPJQBVoaUmSogPTUAONvKUqbVbCFpZFDiyt%2Bl3G8eDdHikshbY2P2LZUdguecqSKDGse0Js%2B5jb5jO1ETBGBgK9qHE%2FYoEEtzWmVhsxK61BkEXsgXF6Lp66ZzFoypo3rK0cYrFLNbrPDkEYM8ipiv3bab9u5i63RJs6887RSMyCL3B6gG5In8cJXA3YbPDk3ZT3IkaEXgdjRLrNSTwakA1fjImKRqEAeEyA93ZEX7UIg3iniVNH8Zb7IwyRIFRGbleMFFPuPxTKM3T7CIhMwgYPhyAY6pgEGH%2FA9tjRxBmNXdOF%2BmrlpXWV%2B5aMaWLeSzyI6Ipc%2FOdw%2BwCfR%2B9gy%2B%2Ftc5Gql3D2fca9l%2FQiRxz5GHTurP8faGh2MtC6WpMMfevsUPn1NNoNeT5x5KQ6ZcLBojOsQMkm%2B%2BlJETQCiMyyCOLRg7cM6Rc2aZLLdvAVQm0l%2BtL1211BS6YMcLuGhwbSnIJ2MfX670uuzGzU4zhVPdh0QKS3bIfQE4yZ0&X-Amz-Signature=516f7047a62ff9bb8f36de779c4a26086944e2d47c781643fecb0ac5649339d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

