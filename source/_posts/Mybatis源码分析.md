---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YH3NQLYG%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJIMEYCIQDdyNXOnL7ZmnA%2Fo2wXKl%2Fz1QLJFzmSMVAFdYVT0PTQiwIhAKg08bQg7yxt39EglLoA1m7YSlw0pWnCkaQzoVhGuKilKv8DCDYQABoMNjM3NDIzMTgzODA1Igyf8PBmDu%2BNjhnRAvMq3AMpnQYgAwDcJZ0W9NiZ8UPnawlJsZa49fS7bMJcAgQSna%2FxgynaK%2FopQcjy7a4Ng24hbaaoTj4a5YUUAPmsyNfMKC3l0LHrtbyMdFWbh5%2FgJF%2BSVE4U0NxBq9UfoJBoPVt4KCrp3Ndf9EfNqhlCn%2BBO7Stv3YeWZWplwJw7NsyMDRlzszB%2BDctFXjU7pif75uRn%2BbEiZ0B2om3Cxld1V0Pe2jIlS70RhhbK5S1ePCYrN7456NI33fqhBnHJL2M1dPNhLRcUmX25NUb3Qmh4eKc7m5kVQc2j1ZFWzoxtEanTNNyD%2BJ4Bk1oFpzBbK4pcznT4kDKKfKxSg8%2Fisyv0tsZfsI%2BrWqjBDyUx6W4i3YODS7OwxSfL7kyy0nw14GR7St298BNDNbBcxY47FXDZhgwR3KOetJv6iagjfpr0u05%2Ba62BszmlIss2Kbxd9ZEpPAu4DpGZ17cWIehnRoeZs%2B1tykEs8YB3t0gRP3UFGTjl9BD16QDwiX%2Fzmkr%2FAvEuKFuLz%2F2l8iwcfRRnvNHLxdmfGWOkeDUEnzMuhhUUrsYf8IZzXecNJYFSy5RA8rQMn4dUW21tq9GroNihGnnjFhcU33Mcl11MmTKnjhW8ohqCaH%2Bo1rK6PJk4fR8hyTC%2F%2B%2BTHBjqkAYYueqriDFIkrZEO0eIG2UYv3z473ziEtJpR0fYHWTMdJrdUBkhtpaD3pib6YD6vde8Q%2FZonuZD9b61tztlHQI63yDNCGCDgmdGdtRwnCmTB8fuF02akSr66G9jvsP07T9se18m5Rdmy%2Bv8SpfFoBEiHYvpxploIzezZ%2BgB7JeuIBmhzuVa5T%2FfsR8%2FjUHyRdV3gb3kqsBqm6P7CyI%2BRI8f1WXuA&X-Amz-Signature=c033c35a1628659e537fc85da48a19423add235f678da082badd12aa3b12f0ed&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

