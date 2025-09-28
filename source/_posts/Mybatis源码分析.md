---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z3LVLBNK%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T220045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJGMEQCIHX%2FXFSPLdv31jO2mMpRohfKwMBwLBTvf8gN3Viu1DM7AiAaM724xRdpOwBW3paktxPsBnLqFijfiDVATCjLdMH0oiqIBAjH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMbjseqRFs5Co3jr3oKtwD20SxAhMG%2FKidR06AopcF8zf8VJNEO84w1vrulacry54G1cOcGE%2BRgrpncDjIpPWbWTooId9mCvfxDaD4w8oyldCgGcJwYhArPgcHNgpWKLJ7N3IKw1CkVCIs6NWRcM1iux5k%2BiQ2xtB3Zt4yF%2FieO3Cva5DwdWTLSnZnAxjLum1ReqASShLpDr430YZxBco3vYqs5Seut%2Fui9qtf07eDQbeK9%2BKuClhidbxIGLeBel%2BtvqUoqnrW63F%2FT3fxkBg2LrXx33gZYnYWE9LC9FKYgPed3nEVZW5nQwtq6zhoF%2B%2FhbSxjz6fe1P7nwfUkl0CTUTqCjnReBs0EtOozPTnKmi74O%2FfMsF4NBJxtWZss%2FYNG4%2FLusDDEi6Q1jh4aDcg4J2%2B7RURV3HMTi4ymkFYTZ5xopnf%2B6MEBQI8T%2F4aP%2Fe2747DW%2FD5UtqtOUsCJS8S8q8FVJK9gCsaa376xuYgfuiWqUZUQ0KcHJKxgoWS6b0EzVn4D9%2FXlpvhqMB%2B0urtJU7dkIJzsnKeolg33%2BKfpVIsMohRBCbG6x21RQIfp9bdYMfkBwR9iIyJ4KhQ8VZRUGvq%2FPICGEtH7SUgNaKhR1PR9YCoQT%2BhwuWGM7mKILuPFkW2Q%2B%2B6o7Z7GB5Mw19rmxgY6pgG3kcCyJnMRANXD6xf26xkuDUiY%2FB0F%2B0pCc8TqAUA8%2B6SA4aWBPMv1dd5PYxfoA6IGX9SRuCLrAAD0GiU8x%2BgG1LubCGzfwfEWeoDZ3WXGIMO5fSLMQhsuoS%2Bkzcrv6VZ6gr6Y6C%2B8DaLra1%2BtQi6VFzwdaQFclKDU5A8gkIna%2BSqD%2Bqrw1UzZliNGdwTmpX2eRHU%2F54iuAkN3ZksmXV35osSGn%2FGl&X-Amz-Signature=05cca01cfc41676d157e24362c5ae7846cd17f8910df28f197907723adc025a1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

