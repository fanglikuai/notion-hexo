---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46672N2A3UK%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T140107Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJGMEQCIH1DUFimS6mfG%2BSVy2il05tJkjjd5NV6cG7sz0nmZP0MAiAEps7vrtWpJU4oQhUDGmFx9VvINzlkimW9jQYECMbvJSr%2FAwgGEAAaDDYzNzQyMzE4MzgwNSIMhtnGCNE6Y4KCGOU2KtwDl7Wsnyx8i25AEgaPiulChX4NyBFFO8ITBP0mWV2BeRmzC%2BmvNMudsQYXnVANJ3nIxGj%2Fg%2FcRVYS7XeiPabjYs3NOo%2BYcuKdkmva9ftsc3pYAtuGCoymytaf2tSPZblVUIftsMVd9934HazBsYty69q2%2BhwwlRV62dl11ONqm4IDRn9XFWUj9ZS5ygfOWEGmZjDMQoPQZUbRON9bw%2BDBBwJw1NQSteJc49%2F9wq5fEROwlc5mzG5W4gDlD45Y8gBEjlalsR66RYQFjOxrVHGmWbw7gRu22egM4wANCHqzY4aMKTZTxWv%2BcAqG32XQKoF6c5NquKnOUQt3Gltv2ZYA%2BIO0z2MVnzARZijUb%2FNOd24G%2FgXVAADVJlEyurJIgNXNKHHsf0hSeJPu6B4mkFttMLB%2FC0h9PsgFPWDn98EGEW0bAbLXQuq%2BWL%2BUO9Z75CRa696gtZgas6ozv1jXVi1HAS6bEFTEY1MqsEtqq%2BKBFtey1FuGBG1hPY1t4Y800HpVJqwb8Vo5493ZmrZEsdoBhcv%2F0XK2f0zBy0BDx6GuBPFFYoBVGndZdueyXmxYwrfc0SgrWar%2BIQz1NxYxmhsLNkKQOSg%2Bon6mXhyXsaxXSeIbXOIERFdoK%2FoSl1ZIwibfHyAY6pgGT3%2FJNx1t369VUFHCJ64LmuaBtXZUQCF4nsLW26wlj3l3o0LO46ILQWoxAf0WAEcS3wD6%2BUWs729RBSce%2FAc0sQJaM%2F0tNJ4%2BZz7wgZfsJElXCHKgQtpAAuV0wyPEiTrjtb968OZcdmlabjumjgOHaJragb2%2FGMKQYR3zlT4p6l5OoaH7MLxXebziPx33v7UmAIk8UdKwBfFMiGiKL1LWBKC3Tlceb&X-Amz-Signature=408a429badc056b0943c5e5d6d2fab987e003f7206a88030a0ee1a973a4b9354&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

