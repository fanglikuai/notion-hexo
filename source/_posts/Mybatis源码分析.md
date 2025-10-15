---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMCF6LGI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T040053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC2WM5pDNlm3BFIcGA0INBjy6IiIdGsi%2BUHqBnhfho6mAIhAMn6IyTpWdxU%2B%2Fis4eOUmxcrAm%2BSOL2DGpSQcaIT9NQTKv8DCGsQABoMNjM3NDIzMTgzODA1Igw%2F8Bx02FOQfXrlQGMq3ANiKlChBrckFM5Ognc9NqQB5UFRozEAkj5BQDz8yafJr5CAtN3t49GKZ70ffgIdjxNzaVyzBvA%2FtGY4uVAvtiFZCgBCy190sa7v2i8WWxf6Zrlruj82WROMDIH7zYnOd67gcA%2FBwvEIMhBv5Wh2mxu337eo7DVI9GI%2BO5XffeuUDTAkphFCiAln6wdRA%2Bn1F9GL9acmJDvXLdm1kPEP%2B7yuZcK2PQKhdA7fLqx5f7YkzL%2FbpEUv5aSzVSF6w199VWVu6jTP1ewdz85kNFdf06oMzoFe2OMhPlERI6vnMxxVApEkKI4j2fqMBq1ggzAMTegqVFCwYmehQUoWLU%2FAYGv2CEXYIo76lvuVr3phDGpcT4Y9%2B7O08IJh6XSSBK9WyfZpWXZ1t9iAyhcYBfrnLY13AoAiIjv9wP52LlTIz%2BiYcpS1cQaJU6pd0PchJOEqMUvHeu21raeCg0rfvffSvO1dg5hbFqxz1Qx2zdEz1GOmr9ICdIUjTx6QceZWq2n%2FHF5X6Aisi0yqMvy88Ns%2Fs6B94lTMB1MYWX0aoJOL2yKximINYr3hTzhG5uZlIQjVAB8RZVYmWw5cQMHUAg5%2Bw5%2FbZExsudFIRvEdfaeyUUyCPluGYZYUXZeg40qmLDCHibzHBjqkAePG10imlOd%2FgthToLhgmVcScBp6rIMVFVVq4ohZnlQb5MFTvkUYOr96tNJ6of96m30%2Bi8aDZ1LCB19yirsHfpZ0t%2FzQYFh9p8kFZ89XOoWpzacg5fyjG8EBnrhCxiifZgVcuxhjIFWFSZGUx%2FDqwFo8BelvBsrF56j0XcO2iTJEQQ66TPmilJ9MWmlLJ737e%2FHyac2xtK5vO4uIjHqLCRvWB5rn&X-Amz-Signature=1ece41c1a21afb2257e8346f98b9d4e32c1fc5e47d1159cab7ad2b959af73602&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

