---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VWKP4NSE%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T170037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIFApd%2FtWi4yUwlw5MOgffKAaK7LnPToSFRowfrDc%2B7ZYAiEAztASdUL8h1Rp%2BUFLqHqdDZRwnYqa8R7wkgPHM07t%2FI8qiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKz1YfXsBjYSn%2BC29SrcAy%2FuosT%2FWavINBmF54LUACJFXHBQlyRcAtXaCA9hOsXrDOwnZORRLAKMsz2RfgzYrKsxkfxXajQDDtK5SO2w80bq7LBlS7GT5aWV2T83twhvy6gAhIAT1YYFLBo2TWmrzQPBpplDPSPYZLvyzBj0JYJG%2BPIU9ZOY6GICefmNT6YhIAbgmJGDsE3SlXKmadQK9GEViB%2B8wjb5Cwq5y9mulcH7zLRBF77oUSaqet4lONf4BSMCyql3%2BYDslb7rJs%2FXplZbcwiAFDEyGIuxMi9jchJG1a%2B7mWJgZpKdIgmVyYlRrBUiUmTcNfBSWuQkjOKsIIFN4I65ArYTyv3fi5%2Bh4i4yPld%2FvBtIE5C3s5oL6wQds%2BIs33RF4QhiDUcoha26MAhf%2BqPt7voB6tz8R4I0m9DkY2tOIgYastHlgj%2BC6bMnB2Xk%2FmB%2Bdv7DblQldoFH0iLaw55NmsnADWPn9X5vwAK55xOMhCki5eHntIgKGegP4XbY2pL7DE0jIEc8lMgd6OQ0IPWrDi%2BelW5KrAKm%2F5TpvOqSGF4uUGHsuTmeUEAVpq%2BT6%2BAvf6lt7Xk40eEilzgg9I%2FtaW1oMZJ8ziwXYbCrVGpWL3msRNLcHOsLkjNpZ6%2FkeS8O8TI1cVrGMOrv5MYGOqUB7tvXISg8St%2FcPjstKJokRsJGDe5OTfYLjUOP1LyIjCh4KVSRTkmczbL%2FchsS32MhtkUN%2BNpDaC03Wn7z%2FYNTwKYKLeVwq1Vyn0BnPKQ3wKvmTKfI7ky7QZUxrcVdTmip93qgIHNpdmPVmbmJZ4R970mkLw5jMFFI0KAE92UziTkyEaD9d2Kx8MQ5dS4gePd2pJOnB3yjlNQk4GWl3rsh3i2Wy3QU&X-Amz-Signature=ea4a57c0bf333ae7b6ec0140e98f5ab6449400d2922b57ca0ef0c863f64a32ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

