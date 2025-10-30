---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663DUPKNLD%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T040056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJIMEYCIQCGHnaD6tXpxPBctFEFA9H3EKpalcofdyKKoLEnZmCzCQIhAO%2F685uAuHHBzY8CWoiS8ao9mbGqr2Q%2BUolOKNjZOaoMKogECOT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8RSiGpZQaAOcWLqkq3ANPp41uob71vCJZsV5dFW2Gfv%2FQ1bASCUmFw5hcJ5NAYFtQRusr12fnn1ZPvNm53TCJ%2B4bYc0JXVM7Ut3s6a4VhxL0VimGvZmE7pMw3wp80GZikFvtmjVQVC9riFrWDaVn6mG2g7mEOGWYj4x7uYPcBkA8irAJXRcu58DF6xxHaGmLtwLYV2XT%2BK0xzEi3Ej34B%2FhJ%2BZ4l%2FbeM09iT9KRFo1Gijr96A8rHLhpbOkAnOQQrZTDwmPmuZMrrcyfP2d02TxbV6IZus5Fb3V0sVCmqv6knkgG9sL93y%2FwCK9jlegHYWLjc%2Bz0rgmXkR%2B%2FU9QCUrWSlOmWsp13zyioNiYpc1qP7vd9qZO2wjVRowmSYtYpcBtIDNInUaWsnf%2Brq2x%2BcbLpB8enHzQUGHULQjJRwH1mTomxcSCYkXVgxXVfmVPXoeuGdG8Td3%2BPzci%2FGLG2s7hCzpL9BmfY7skEgD7aFjx4hHuBxgoLGJlfCp%2Fk2rWCk0rI52JDnJg60nmhAMDKqKz0%2BnbeCPrpFHfmdKiDxGiN2ZJH1HT9YXV5CME4GbIqbTAlS0%2FWuCFVli7VFvQnwwtpIQhtRd0lY09Jozi0%2BBYLgAlC5TystnLupKFv3Wj15S%2BcFKmXj0jmeEJDCms4vIBjqkAQZwK%2FggCNnBgCY7RFzEq3kqJM5%2BrhC45H0DE4h4dtY9sOi6%2BBmZW9c7o1ksuIU%2FRtth%2F2mu8nmGu5tzl6u%2FsJNlSSB5haowl75jPMq%2F%2BXhXhFCvKM2oSvH4qvnXcnOP3aRFeVD1hljrXLBVMJ1%2F5JufwrnYUyI%2Fz41yDQCsn2wq0snH1D7Y6MtaqQi6B5fgUJ%2Beqm%2FLGgEqh55RT2IaXoYIv9%2Bx&X-Amz-Signature=f0c2b2c4ed71c90ac372d9f7b68c52e9620a64f1b0d3f5966cb33561a0a6c0dd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

