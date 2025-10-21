---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46625ZVABFY%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T230056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGcaCXVzLXdlc3QtMiJIMEYCIQCiFjuSe%2Bq5hXT7F9Ij3W33OIYGz09Wv2zzdwWH%2BCav9QIhAMlKP53VeEy83mh0iiTZsnNjKN1GmmFgjaMLlpTSeBHBKv8DCCAQABoMNjM3NDIzMTgzODA1IgzJAdfBODK77NvFgbkq3AMNSS4lAX8iuafQEtJVAPajUVzqdbuf7wWeXZzLtigPgUqTSTQO%2FGCJ6gIJeYvNNNFlw2B297iwVvoiggKKmxZlLD%2F3mjzSUaMdnkydlDQkOEDVJuzhrfo8sHh8ItRw04yKyg2d2pSYF0u6joXkqkd4%2BKxMqxx284QJzkp8vtc5GdtzA%2BMy7hxX1e6SILnThZQWE%2BKjRYNn2Z4i%2BIIHIf7ZSu5EvMtYYNEZwnovSpBmlEyzsFySfVgUq4st3i0IZ%2FFxEey10JfSzlqNEbzb4GRL3x%2BK1LM5WbKv%2Fjcyo4Eks4Ph9QEga%2BaSPHGd5k1FLKm7nMUGLEOTqj2CxPk%2FD1%2FW1X%2FxUwXZD5yrOnz4ybKmrPOHPEUsHCv9ryGToU9%2BIDDYZqXpB08dzPcGD9sAZbTTatjo0Y%2BqXsaEjcvQ91EWOCXgeeM1iIOJ%2FtTcwS2YxTCnfHOgdeQlHSkcIDgGC8OfSx3ib9%2B218s2eA1O7peCJQn1sSbO0Gb1qZaGYmucS1xMYHFhjUWon9%2FRjLqfeCcm1AsKMdwp1NsaTNb8aNxasEjceOezQnk%2F1zE3zOn2I7Wq3lZKtV6BxFXmNtjtpuSaq4KZye1J5X2ssLDbl2T2tLzkprtLBxUhftjdjDCWl%2BDHBjqkARi0Nh66MHwE642Gcxjsg%2FsI1wmWLo%2BwgPnTzDhakOmPtSa4n5KDfI3uR%2FRm3rhBNaYTuvOyAn9wh1Xo45f3R8pypQnnytv7CLpbKocS1iXP0KIDPg2MJuUG0qsl5wobbVKibjWOVMiPOoWWhGcm%2FG3vASf4EHx0kZAlpbIOy4B7ZG6gFWeISFFoIUgHYVc80tfNPX10%2FWIJLXZ6oqsR8rhzcJqT&X-Amz-Signature=69e8982c2bd08e8b7545b8ffa492af71c0b1d20f7e525972859c5b2588da424b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

