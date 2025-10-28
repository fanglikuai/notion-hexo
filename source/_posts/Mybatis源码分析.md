---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZMNYMYRL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T160050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAgaCXVzLXdlc3QtMiJGMEQCICbDrVjXZF%2FfRpY1qVffhF2xxvD4yjyG9u%2FGQsnNCKPGAiAUyMuUjRBDAOuobZN%2BDvhgVMNw7%2BY6q4b6haEfBqLPmCqIBAjB%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMQdgqgy05j11xDHtaKtwD77NyLqb74mgo3c6Y9%2FP4w3iAUUo7gjWZ8I0ZiYoKWes6TLdcNKDsNbUArHYttfwfQcUe%2FJ6XZiLG2bx3LRX7HCUredWbZYUZXABGJtH%2FVxypqnud8PulFDXOLs8IIpLC1%2FLWYHQMV5DBuPjbVkRRdfdqbqTRZGA%2FgVUNscnJDrm1CfZStHnR7N8MjJ1Yu16EgK1hwm7xF9kOT%2Fq4UA1gkyzaxU25TOkY5F1Ih6sP39j1uzHh2nZp5lJgik7RAHgECB4JgItlOorpFR5zK6vNKsuPWGOZoMqDzeJoN9pBTUkLHG%2Bc7JHFKol1Wh1YgdJ5RsSwxKR144MBZf2S5nhzovcUVX5U7PIcSWhoFgqYbsywyeXqnD9sTDyOch4Vk7LkMcoWuHNynb97%2FgE8MhghEeGmEm%2F0YSLEsjZUz2XAnsrvBpkTf3fwRafAU5uPaIGP0Kl%2Bw5v9mjs04aP%2BOzOlIpQxaaQvJHceU5cRfW1yi11A9%2BPFtGlofSnp0oeQmbptCy%2Ft%2FKw6o8CN1SxAkWx7ksgp7d3uWmgelaY90pIXQztC455ya8B3ol1wIZknDZVZcXBb1gyS3M19RSUPqIFGBW21B%2FBhepdfWh1UMB7BvPDlGKck%2FY0MbYS7ynww48%2BDyAY6pgEALf3WhNdzvALJxt3Cd%2FAg26jVb5L651Lc%2FB8Xqo3t3bKqRF0E1DfLf53DyzR15SroMulwI%2FhmV6o2qosfJ4qh8fFmqgw44Zgk791hmo92N3mxlkvZX3mwZ0R2BqhcLXIZ7yY1Te6F6DpBae6Wp5r%2BrqtO8VX9RqNnQoSpTVKdfXL9djYMozTIo1EeGwPV1su%2F%2B6oizL%2F5XMary28T6hxyBvj919cW&X-Amz-Signature=86688deea015ba6e55ac478a58619db84cb87ba188a052fdd0593c18420bb2f3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

