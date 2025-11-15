---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666ABEJYG7%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICaS6Bxq9Hs5LFCVBMz6SMGOhDbijVmxCQrWUF%2FmE8ssAiBelNKqbAWXmETSPGM9Q8KnX0fiL0vQakwc1eyYqbkGCCr%2FAwh6EAAaDDYzNzQyMzE4MzgwNSIMQBPeyy9FKnwnP7%2BHKtwD8hj42BiTATMwAckDWuztUhFp4RVrkEErR%2BQ8avd1ojjCOMrJDQAGtJiCEhQRjargRjD3RP5U0kiRSGT25Hx87V89MkwR017Ra02A0CSCh5h5CgeUcTVCpANlI2evE%2FBYtO6jCreqFbg30XqWD3wWunkFmPBEy8Q%2Bhv1H8NZIJuRAT6LoKJD2cskQdP8Hg2ebGGRETT5S7a%2BKNpHKI00nofA3r1yd%2BiLB%2BkVpkqwXI%2Fn8M6FtDum9F5YnmwwCoZUQnRRyE%2BoudwWPWz7rzinIwDfOD3swWxig%2BpKBGNg4bLiPaA4KoTjozL06M%2F7ZYx03Ics8MQGYf0s3ppahHYfcC0n6WI8zeJNkbnOi3MyAscg6uRYpCS4GbP8nFI6NTKBXCoG1pt%2Fl8bByfMdKHEkA%2BdymCYYHHQGvUm683jRZpnCxlRUzlIAB80AKO200Fe1cuLjMToa742mG7pMAZrEzPj09gpOZCzBCWb7Cz%2FLrxFIgUrsfiCbX02cgIQqbAER66O3dog10IXR9%2Buep9l0YkKXeCgrvjNXjBpS%2F5dJEP%2BJfjepz0AZWSzClC4que1sFisAotcEBc5A%2FRl0cLeMcHuRVnnAMsU3um%2FtI%2B776UNw3gA3Ze8iNp3%2B6FBwwmYPhyAY6pgGf72n22xfZvH3xn0L9Hapyk2rsXNM2LNdwCdNdNg2MBvzC6XhuaDS6S7eVoqwyjBuu5KvY2rN4PtlB6UTMlBDuLefkZZSOl%2BuOorIyw8Cw9jD3GzaVPIMmpZJa605tiK5E7jlgZV6HpbgKUg4TGEDyqBSOdV9%2Byl772ESqyQxnnx5dYx92QNoShDANftPHVM8R%2BMyA19vyXewjmNr9TG6P%2BfJqKlYB&X-Amz-Signature=9f252e0a07dd6b307b1bac66100b350021e0bc74bb955f8aa1222dec31f6e828&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

