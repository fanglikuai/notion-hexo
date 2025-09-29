---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UJCNDLKI%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T140126Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIH87l4k9CBKtzNVYIZrkg%2FYjJO7NK4ymVJBIPn41SNOzAiANH4yPHCygXpOlzk%2B%2FIEntvQBTqcT%2F2cLR7%2F9grIIkVCqIBAjW%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMOW3qRN%2BCzz6h3MHUKtwDmi9JTO%2FhBrz%2FLqQ7NIakWLVSVBBXBQl35WPya23hXIzTSYmwjFEzXSZ81luOQIDXdywORUQhfXPoqY3XLURYqLaBqyKe6FpCqfNyqqQGmSUXatvzkFSnycjh4pZZBnvfQpzUaCWkmHNFm8FCNYk4Fo8W0duPi55ezxkd5C5rKJJ7I4bxxycv7MGpiGkNcVj2Wxt%2BRx%2FGXpwQC9CYb0npGiZYdGq8zA8nKuFMsNHO1XgnDDpnsX9HWNO0rgSb2snsxNwAtaom4EC10VJ4Tw%2FMdYqMr3Vejk4YDxEbB3o3H9A7iu9TYxWSPVqdpf2J8QBw13hKYf5J0d3Tuyjhv7PHRqbr6hkhIwHK4h9J2Nr3KM%2BPBDjpw1YMfNF3A%2FvFGsbFP7c0IsM5kXmRzQEoft3klq6fhDdJTks%2FDvuvrIekR6FqT6kP7DImv632PeBVtNTWwzK9tuiOOfuT4WcInTsgeWg4BSafKmlyh1ekpDnSGFcMi5DbS8uNTXUD6X2RM56SYeZWgkW92iN26kn99c8FfbZ1PmnlIa3uX0%2FYww%2BKMyEg%2F0A0KzJYDigO8wy1jzQ5pn7%2FvoA62zkPeVugfq4h%2Bdv18IEyUfHQUfCgPtR%2FySr6DZdqeRyVAhW4hrkwyYXqxgY6pgFJ7XAbqKFag2Dhp25ht%2Ba98bDWfYPjKRvL%2BhMeNlnU6mJhc5Eznqk5vq0OU63b%2FdwtQzBWNta6nopL3Ht%2BADSfYalPiLNxWUxXRr%2F90I9fXK1maJjGRBNnYMGeQSdfKtJZ2DMyfpkiS%2BX1zQxNPTCd5PhTqpJESgRBS55YmzkCwNEeKfkQMxSn4nj3FLxm%2FlUoMpcBi0pkOG8ge%2FertIoZ31QN%2FW5j&X-Amz-Signature=24e6c978f40c625cc2875e16c87cbb134b47e28b4c4013575ade5dbbe381f861&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

