---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662HCSQQ2A%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T230038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJIMEYCIQDHrag5Ka4Hm2R%2Fuv4lLgXTHFFYCibuK3b9jT%2BgXQx19gIhALAHJJVz6mZvWyo%2Fr6JT8JwfqDctql7kjV2EHJQ9oeLoKogECK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igww17CSBOb%2FkmAZxn0q3ANPhnfWCNPbXQQrHMTyVO54mzMtZxZu%2FXqhiySq9ZhiNR%2FGCdY5qNLAQ725SbUa8JO7INYPcdja1l7wRquM185yGuOZQDWNoPd3pTUF4a5%2B%2Bi3M8SzzVPbeXX%2BNueoW%2Bwb3twNbkdTa6wCjCmMvXmLPIz2yMhV6qm1Jd%2B8tiJPy1RijqSpaGJDBipHvaLPdeLmsVwOxM0LmDpVZCr1NY09sXHk3GckzEiwavXZNTL%2BilpGawRsD41HQw58pKOxu0H8HE%2BRoFkUTfP727xjcPeQN2EGyENkVCcLn3w2ETUeIJPrnma3%2B%2BYdou6glPrkLECzjQAzNizatOihzTWYScZAbgYr3IxhhuQ0nXglZu%2F6wCS55VBx2K9GUvNNFtUz2rcZwvRNwTSFVCySN7qQ97vNUOXJJOOadaa8Ok1xga%2BLksrURXie5dFH2CJYKoYV40iD7aeih316JIf%2FtCBUISAmyl7TFgcOVTw8oQ4d64PUuEpl71NZP7EogfQAQRLCEpzsaFySP8iUm0qRmOAaEOfPI6T6jCaYJsZE0G4dOsT%2BW44db7KMZFYObSDm8j9Jzi5%2B0jZ%2BfI4keRw%2F7YNqpN864Uw%2BCt0ZGJXdzr5wTYM28ye5WtvVMA5nl7Tf8aTCg%2BMrHBjqkAZ16JDKTmDUOstxI47cRoOm16AsztnoR8%2Fjn3EF09l%2F1PyowAYLhbsb2Zuz%2BSYjDNpo0QoCYsFDW4MiNscp9Sy2Lxpi%2F0WhDXIhf5AMhtUpBSbo0%2B9VTAv18F33mdaqMh%2B1cGu7Ljq5YDJEraaZQmhrhYiaRmw0%2FekDwJzkK0YYwuABeaXtczRbNAwTubGRvcgp7UH7byNF4E%2F50ezFQgKQAHuYL&X-Amz-Signature=77fffe09ccb1e0864530bbabb469337538b69d03a2bc161d3f5c4ffca0ad36c4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

