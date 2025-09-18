---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VUUZGBEV%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T160049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJGMEQCIB8c6npNETYnc6vS0syc1yzUnl8QdobfW0b9Kp2uofSlAiAyROLtxaUKe2EoOpId1FGsqeKigpN0xTEDGvB9ff5pOSqIBAi8%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMs9gyXkuFggnsJlUCKtwD7t66sFHhVDYSVRVAjbCQiQWVDkfT5AqAdm3G%2BgdSObsoUmaXgLxs7VyNUi%2FF9aSfBqJj3qezKd6mnoqKQ%2BwksKzTYcnjhonbzNSgTl82LtK4txI8zu0N3noD5tOMfZvQkacAExnKPVCHkWNOX3Iy1ZcB0MIRoAvoOCPvxEnUk%2FUg4VkFmVJkNGmyLEmlrvmOD9KlUzJdRdNPKCvoOde30fUpyVNFhqqwdfCy6ZunFobIxfRWzykJohnUDQtvRSHCc3LDpWurs6p8XD8gq%2BqInEaufOf%2BWnwdCuj9xFRCCehUvokYfaC%2F%2BKgjiD8h%2FEyxR4PfekI%2BoY6fYAaAggpdv2Jq5M2KRayruwjS8gpfN1tjZ2cjEavBpv4qfvq443lJ6UJ6Rh5uz5wlYwiHHeYjJxctX%2FajKPPDtuj4A4%2BBWR0Yu1HMmefys1Bw6F87PBOFXkOL%2BbJletMYWmFarcEo1Eytm8BX3xF7kTg7FRF2wm0RawUB7pILndzY%2F6CrHPEptP3wzfR%2F2Hbz%2BOEdXdvSfuNcBhDBn9lP%2BYHeE8PUQoYe4on3XoNb4EZT%2FQDGFavqED%2B0QV4mqSJ3WvMOghwNZtRjozD6nEfuNthG0b5G7xMYfZC5VO1Pv%2BIc7oMwltevxgY6pgF7M0j1RlJZHciRtvVQZH60p%2Fd2P9njIACBy29y30IZb%2BqEHfXNGw24FHgKGGEDIlBp1kP6S%2F8a4BROfShwN2e45UjftXfDQzXbhgjvbe93RUJcJ730eAQNRqrkR6dAKeM5nfF%2Bf1W3lb26laNx8hATmXXUIGm86mszksdwiQy5mIBLxBAFCFagRAK349tgx%2BsnN2khz2FxGYDAymAXdNCDx6Qqx8VT&X-Amz-Signature=a1c4ab031fb72c812f230ec45eb54d6169f8992866377b686aa0afba1361619b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

