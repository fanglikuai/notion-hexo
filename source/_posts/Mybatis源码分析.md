---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VFKWQK3H%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T190041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCIF0CJIByZ%2BVEYx9U4DiE3KaOrdZWQkFb2%2F6Nm47Zd3GIAiA80Sa9%2FIhPvuKc7O%2B6W6QNP7V8uzziRGZgU22juu6URiqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM5shLJsFXJiGsCoafKtwDOqg2Esd7NSFLu8ebuYgsEdiDA2mzBgAvdcMP4%2BbKfz7yukTqWsTVKe46l9HR1Uy0TDOHNuP2o%2Br2bxjbXffvaeRldMn73QZf%2FSV1OISTcWhbtFnEt3qr7NB00TTH07CqpIcCKrxPD8iAnk%2FzfL2Tpr5ihNE5LrxMZcO5QXIhxmKa7PadSNmlQTxJjMLimUQPbdYM2nG7GREvRMNDmEiin2AXuGLIhBetHxg1O6RETfMPNFKhmyZ%2FtbCKE9afXVkAhNR0d%2FLgevjfHH9lbjPjkOaRc2SvUayVrDvsPju9bbxCl%2B%2BOvf%2FiEZloDCO%2FcPpbrvnkGQjhZpfZ2AA9Fumhhr4JuArDzIAg7pCg671Dtkm%2FkMDGoeMtccTZROp%2FB2cX881iS6o0yK4Ll628UwLVIo0FjbV5WybR5XERoh%2B7nR0yZ85M5amPQiqGAgrjQMe7O87IsPH%2F8QzNu8GLpK3BPSWrPlRfu3aHfIK6wCFNU%2FYaNNYkHhwGFwRaoqFodCFz9dtvLQ6L88s9P9ZDOKOvUtuimmSr9IoCUPY0hV8jSRMK51deNWrOBtVWx3ovcCfpZHtr1UQ6goOa9Y4cRXsP%2FqBGOedu46pDJnuljHFL4TssiF1sHhbFff%2FZYzUww%2BfyyAY6pgElHiPvry0I%2B3MX3l7fhyJZhM%2FbQZ3wWj1JYQ0%2FM58nMM9jZQBG6TwheC%2F0lTRjgrlKPNoWqvadN5j2USp3hjFCmMosJ0D4hgzYx8Qld1oJqCHdHliG8s1HmkpxGgsTN4%2FUkNh8ntvBGrmsCQdtzZSX9qC9OptHmFeKEP0vvob00v2%2F8UBqTDpDL41x3tARM3K%2BQbSBUAK5ewVgZ6r2LpdJEEBJQIon&X-Amz-Signature=4e40cc94da0e04ead8717adb7c92427feb02b769e07ba6f849f6a79135738282&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

