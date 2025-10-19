---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XQXINEFQ%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T090047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECcaCXVzLXdlc3QtMiJHMEUCIQC1ExpW93gOgl28pcKLXqvZVv3A8zoahByuTZE%2BigMtIwIgdcpkuY0jj57FIZgPRhaPBak5lnncfQ4HOk3sKHZWny8qiAQI0P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFHNcuB9TGEQTt2%2BNCrcAzGwA23CPLcOKnt9UtDdMDQjo4z6zLdnil5yrH3%2B2u1ndnypjsoTmJXRvlHT7eJ4T24S0Mqn9mjmtHTOlgis%2FALKN%2Ft5dV17GUDUKN07zQxJco%2BHHqSj5aGoJueCmgzBvjTCaD4w36KJJD%2FWl%2B%2Fyj%2F6cxGbvU1BbIBzcxZKgKwcxtgE%2BIg%2BM2I1q2D6KRpG2xWYC5gdTzsC9IdcY7yR2VSktsrHM44MfD2pV8SjCc7lHtMwT9zjMcD9iuSjplapAk0JoNXfx9paV08L5fEODN%2FEj0sKf796UALPdqtBAgNp%2FIZZb91zBjmLQXrCcu9Xb8qRq%2Fp0x8UT0Wox3AcTCjqqDkTYIpITIMMNiS6M2PkrLY2tvIYMlDWtLhuVUKIaw4Tn%2B5SwXztxgqdGlZcIOb79qW4vF8qVIMBWxg25HG3V3mjh72fmSi9tpSao508gZ3UNYOrgcSlUxiK1E67Nw9J8qgPPhjri5sFUpEof7%2F6MRbZ%2FgrsYOobZnmMmsCZe3JGf2%2BIS%2BWv0cgm7PLBk8V0PkxQB0xj9qrx3QxBmtLHSKQf24CUlwOv7MaE5JzKzLKY4MdZAv3xS%2BoDskI4WTcONKu5nBiwn9S6HF9cqmkWj%2FTlHmR0BjsL88w%2FFCMICK0scGOqUBCfj%2FCE5b%2F1NDk7Wrkq%2FWJeVvE1OgEZppM%2Ft4DJi13sixfit91GJCr%2ButkzOlZNKi2uhl%2FCv9s8McsajuSIXUqPxmDqA4EP1eyiHm0fJZ%2BuzJzIJnLU2a3CcSqTny6xNfVKdqOjyp9G25K8WzWRAd%2FaTof1%2BsqYljjIdgcIQd7rl0BciAQyubvavAANdU8F%2B9F8KEDnJVX28BHSikxfhtDpRcOhSR&X-Amz-Signature=bd62f5f072063b52e5d445232fc4d73026d1277e1de16ad3853dc153177573f8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

