---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDB6DANY%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T140040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHEaCXVzLXdlc3QtMiJHMEUCIQCF9rY8SENEtlX0NRGrKS4lJm9kNhOBl7vODMXcBDUcBwIgWViHW7nLk4SL%2FE9lZjUXCBHcWHH2xxyyL5eEtHrL9hsq%2FwMIOhAAGgw2Mzc0MjMxODM4MDUiDJy1ZmuZVj3Weg9dQircA8ajgoeNjO3cLTww2b%2FBKtgcgmK2z8xLrl6J73chyXhlcRihhB3BJvXmR%2F%2FGCTuRUoo8JOkYAMUzfq%2F2mMW501hJkyEAQtwSuCPi3OBE%2BIIYNP2sFaNfQ%2BdesLjvGGGMIH1PVuCz1LD63qG7hJzEwpEETHq2C%2BefcjRQUqOHkJxkG%2FA6mkmk0lRUZHflt0AaMMma%2BenffoLYXt0Wd%2FoTJy%2FB6xM%2FkNKur0vCjE40I5Hsu9UWsHmbLlYUtuuw0G1MY6fhrWqjI5JtuuOPf41VUERGHNSLrfYT9yQsJly7vSsASNC68HVWpTumAf3Ns%2BCxTc3jUXu8pYZMMehfNsf5anyoToV%2BS8GR%2FLLBW0QRQiURDAWqgPYe4CmwZdd%2FsvzYLLbR6bu%2BAxXbhN%2B2TOWDABqNdY0QRbCe%2FcjMvEPAI9Z1P2kUwpTMX1ysslruiPwYNYbOJHM4vHYFa%2BAZ3ruLoxJhTj1L34t%2BW2f5KLadi2jKC7cWT35MkaxNocGGqFOHNRLaF6z%2BK%2Fi0yYtqoHrK10L%2Fm4TR5yx1jozK1CI9NmCWgj%2FSI30peW0vjU%2FxgNuWOYRmOy2vHclx4QgZPLwqbmmcb%2BBdXw%2BYlfmlD8ZP%2FrcgQiYedgZdKK%2BqlQAxMN%2BWi8kGOqUBhe%2BcvfTzReIxPdNxXTGM%2FikHiPNlPHYpWnn5EVUcVm2KNHTYniHzUmolrYPR%2BmhsJEA0dHLab544Nxw1KeE4LNFTNUTNm1JX%2Bag1J5AbCnN5rBGLWwbcCqOG5RQ4XI3rcpt%2BESLbweAj9S8OcS0LZG%2BsZ3lTIVIrK%2FcSZA6YfYxSn956YdBYNx1C3rU%2BlP9WbGiqwKPkgEk2pN7ye07KJCE2UDAZ&X-Amz-Signature=9b97e832af4c012ec73ef24406cd4dfd5fc922988d6b83bbf28cf64b9ed371b6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

