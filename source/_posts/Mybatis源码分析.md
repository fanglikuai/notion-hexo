---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664VVG7IZ4%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T150052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFsaCXVzLXdlc3QtMiJHMEUCIHrV6zfO1fruR8Dc0yskLzEOCARPDbDeoIZ83GFK2DcMAiEAq88S4gsZdY3Bxn1uM5rpXedXSWNXlO27fT6B%2FWKrPmoqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHibS7pCAv%2F3pHjJYCrcA0fceBNHDhu6hNO4tDIBykgzRONExPcxlJ38fHI2TLSRvKmUiId8VyyciNVPPI9GDm%2Fw4B6F7vl%2F0Enh2P2qlJnKURzCyAjF51ptD0Dt%2F4NO%2BT4ymQC2PUyD6ii0ZjQeD4RVn3trtZuyPs143r5jYpnmIZSFBUZIH94YS9ZJDCjvGLpwTEzGbCl5OMnwsebDeu7OheDD9l%2FILajZPlfDORSNPSDRaxkSpxEabOz8lnRRT0o3QegeEnI3z2gmY9ccJ6Fm6gyJ9Xc%2F4neIEt5aWh2VSVIK1XBKZ46I27lKjkay7X6fuiml5H5r%2B5R6fZ0ACBrFdn%2FhrRo5YoKEuubC%2Bbjqrzgk4Xam5jB38A0%2FqWZxX%2Ff6nmjq1mbqPW1XQQ38SaaqapOL9nEKhTGV6kQrHHwRzORaRe6QtONfPW7LjUSsXWE5mTbKHcdeMaCTfEYbNf8%2F4AwAAO1UusvYw0zaVNl17B6FI%2B4xDsIfQno9R2S2xSGzCFPTwY9GrnveBWegCaiOxb56V1q80wa8U%2BL86nLKeX5j751UZAuJ6cR%2B883ofPu8LoCec5Qj5qTT4HJNZGIgqrKbNlnKuZ%2BDWJiASCuP%2FiB45qOXIbWQrkt23x3y7VdpGVL1i8pemzISMNzetMYGOqUB%2FrAi%2B043It%2Fp14e2CEj2YjU%2BfaTeaXEl%2BTid6RKJsVIktYhIEHulT%2F3bht76VSiEmWRGlZHw4ePOsElDG6Ezj4KbqtnCRMtaC0E09%2Bc47MoGUbBsHXbiDckdn3Mh86Gwg4wgOvrysxD7dD4WvCyBk5C6dyZserurzxFJxZpHPRCZqybU4cq6hicTjwzn9ygwYuF2oGjp9pLK3Z2Jh7CTL2qGZyEh&X-Amz-Signature=47ec9a6f080a9bf4186c3820de6ca86c4a9c43fbd985f95ba8b12ff87146ad99&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

