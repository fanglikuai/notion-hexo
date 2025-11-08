---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665M4D7AMF%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T060052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAYaCXVzLXdlc3QtMiJHMEUCIE8GAcJPKlws0QS5GAmEJIDEhYAK0JUzwMAeh6ZTSnigAiEAkC6B9xhuPRLT9BEsLXmEuJit8e4aXhzM0CxEvvbmnr8qiAQIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMurD4fXq4bqCO21iCrcAyT9i7BePllSorrBMc3F79dA9Ezdi9OdCnfSzzvckjcuRr9mLOAxXPcEe0rmLkuec3%2BTuyKNANlfhRDf7vFos2awHvzu5WGwzyjEroVpvxX2m3pMnC5eljiNNBjqFoumwiZjk1HForMoXAUJRnaW7LZejMADcvyOKJ6w3MPNDYanyUW%2BQ9Hz3ld10%2BA3hCqyZnChR0ThP522fDzpOrxrQ%2BIApFqfS8FPL41EhPGvmiOEWJhbigadFpyQ8ebHxZzYAc%2B7xbXdCb683SRnH6k69qGnPicD3EmA48vOqs1D%2BRpL2DYoru7%2FXGT8EzVsLhwpWdkmxSpB%2BaeHzUY2c77MYzohhhEU9lQ7C%2FbZqDU8h1DXTi01PWO2FRA0X%2F%2B7NCePQJwPNWjB3dIU1nVkRZyC1TVrkkd485af5yCW1Yr5STBN4X6c%2B2CFtg5yvFVPDZr389ZNb6Pag27LEehjxLACcbUBNzRyAadFjo7AU3mvCbYSOLsY8SkqWsjp%2BBYodcJy9GktcejZYRudK9fwOGSSFs3ahF8XTUwMz%2FcgbXl3r4ot5oSv%2ByUXad12diGUOiv2B3p3QmVtMPEXzRQp8UQKXyjQIwqGrouxaPY2xYuYN4x8YZaEL1WlDNb63a8cMKuvu8gGOqUBhLVEPQRld3SIBNwXItgvnzEgayxm1yMpbJ8nfyxYPW4%2FN78R%2Br4q%2B6jGFgvLKhjXMkCUeDU8%2FABWdSz%2BSdBkYaqMePhIysX%2F0gL8f4r9I8t%2BaSCPTC5Gl%2BRHNre0JBvixZFH8vWi0Hi1R7QIS0Fs0ch8SdP%2F1m0OOCotEOSt9%2F8%2F2MMrxq8MLaE1FGD%2BGG%2FY5ugfBuNx%2BsV1huk8jujzQ2cwd7nl&X-Amz-Signature=847be9955185f9e45f0e4cb631e2d2db4cc6dde0848813c6748339b396bfee58&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

