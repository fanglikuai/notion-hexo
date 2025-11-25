---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46643MMB2HT%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T190044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQD6QCeNfP7RS1aV7b0NBRZQyjNW7BSeHj56uhf8if9HQwIhAMy86iL6bLnsgXmPiNuvbCkjTPsXrmhSAY0KOaeESlgeKv8DCHMQABoMNjM3NDIzMTgzODA1IgzAS4SXPJ1Jx4Yy56wq3AOMQZcsfrqmht%2Fgqalr33io8SDVXP0GiRaeGQtxYmw5bcrw3jgrTtEOJlrsYZHbSZoyVnFtoNrgg3qOmI7JnV2XGDyJt8dnbWS3zxbZAvURnpOPSMYwr73IwtebWMsH05vwureEvInTO3vHtmOVWM3ah7i6sjZsiDRWt0LNEHiE7bbwyECiLLXy3QeD09IpipH32Efkz9QjNr6SMGx9oAKHCFD%2FqlPsitKuu8XcdnksoCVbiGv4uZVOc03%2BlojQrhszq58OpAo9VDXg1X%2FnGGj9MtsMKRqvmMP%2BE57lVW%2Biipui6ts%2BjOhVaBF1p9UR06BHmXrmV4Gxf62n75a%2F7GIVdRC3F4K61KF%2F6thKXSQbgp%2FjsHW5Y8WsiAvdABSECh5NaV2D7FnNRP9D2JngOhZiI10imNxHgA7pwhXGAwujlBSCocRxladbwtSniWOzdxfMpcI3cVoJoj9p7sdzTPyxPg3j%2B9dcjccFfjv%2FHi2DgLHlVvq%2FaE0Vs%2B6jlA%2B1bRWfDqY4HP07%2F1KKeylG74Nj9YAg%2FB3%2FQIfYN5L76LPPi3d%2F5np%2BbyKJOUi%2B1x02NslY96J9dbCSZKt7ZEP6pg5gRgSl1V95BLwBUvREPEH%2FF6m%2B8G%2FHcAToxn8D%2FTCu1pfJBjqkAeoxTPd0JaohAB1Nxjj8Wp6jXEpEZg15hEsWNHshgW2hDgbcuG%2BCA2AfFiBBZWTHcKbEsaKcAJOZRtKe%2FI6nxiQIs8K93BKxfSY84VxceCARA2bIDvXtAy3KCfm88IFzJdLKDhlVAweHNJ6vhrBTGmfFFyfD9rEyPe7pmyEfwuABnRlBlYgqGHDgcW7kiQHWNkweoDqY6dvDpHSy8FiMLl%2FtfZp9&X-Amz-Signature=0e16e72fe7e86c765c2ebbb8155a1866e2d1666633c2f30eeea2d8e72f59f71a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

