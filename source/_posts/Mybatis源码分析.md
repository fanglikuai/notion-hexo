---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZIQUDSJW%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T100044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDRKxi75tXMheJrLS0QKbpLpdfe375%2BAWQcz2FHDl0iGQIhAOfc4vGQKpGByqR0XlaMk%2F07pIh7ZNNWLouHbO8prJKMKogECKL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwy2Gb38awfSTtkPbkq3APu6mlkkIlM27sIMh0GZ8I5tlLgGh7C5aJ86dqfCpXS06wpPpmjlj5chk1SrRxFq9CI04kOfhMkWWvl8K68jIpxqjd4or1Jpk2j7F27G23OFdp5fgD6x0AMUoFsmEbkQdY%2Fk7Ta1H7UNKLYh5z%2FVR7D12K2qLGiWs6m3EOWHKtGxnLe6IkRhydehh3Dflf4PpMeEcbLt2zTv9j2ZxoiYX%2F3Fu7yS3x5DRXQzBFsJum6RWOBB5MmrmBtN5TL%2Fgy45iIih2YUOy7zCjJA5IJErDM9T1ZN%2BR4C1wfzfhFRG1XJDGy2oA3fFSHDY%2FTATwPtN8TPyusiR1yI6Iu%2FZ1Y50EoYn46If051QcK0xHEk6kV6xLoKPwEJgwfQkmReJicuzBS2zLkgm4GT9UdHIxoolVkqPqwsc2Z1Kx79%2BOyCLN6O6pghe76dieMw%2BRD%2FxxFvVsLu5avdfKqWftVzSLe4%2F%2Bh57wZ4AAuH03LjV%2BvGZOGhtdBxDPiFHVvMQjYobTIDCCGBJu3mbDG7%2F18eAmf3GuMG3bHQwm0kcnRX0Y7J6rJAS9UphUr2803u8byNddN%2BYPYPzlTUTYv6XYWdMgwjlwDLGlEgaVRFpCVbV5G4rafFCg18uABi4gsXURKtXDCw8%2FzHBjqkAaIBMDuZDwPX74D2y5qbkLXNHId45E%2BK1%2FV7SLymiZBB3hFlO2es8FGyLyiEAAoXfdGZe1CjDQ%2FKUnr0qHtdAMdW3FH2%2BhHxtWNH8VS25R%2BFD4jOnClG6VkUSyMusHJLXHGPFA3P%2B8SJljhlVNv00Vs%2FgD%2F0DZrYpp78PA7o%2FoTSXLdTWEvwgzWGZPaHXwCccQdFVQ4YXddbbF9t%2BGpQ8zM%2BbX%2Bv&X-Amz-Signature=0c9147ff611020e80548b87721e77f4986c35ee304a51efd9460e68bee0e9ba7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

