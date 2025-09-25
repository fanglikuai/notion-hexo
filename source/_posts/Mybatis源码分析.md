---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664UZH2JZR%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T020039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCBbzGFtsHj6wjVupINuzSlIiYeM5krY2T4cWsThVMAmQIgWkFtHtHOqJAwwoXuHmTQdmHWfmd5DMw2qf74vE2VgN8q%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDNocZGyDL2rpK2fHsSrcA%2FJHKhMy9imEL%2FFjs8soZThKZHwvv%2Bkthh%2BEvwAhEqnIJ6CMl03TcnXi%2BNchWQm6WQjctS2BMi6P1PwNaIoZTeXwBd4RgXjToYbSwJBr00Z80S4Ai02gIvrz8gOugRO2xnz2%2FrKthXfUC6FYc08JvKEIvNuUxDOdlKl4R%2FEWPJA5gp0wu2xfcxCRJUoOi7ECztMa9msE%2F7g%2Fql0HEKNZqL9nqZCEsvt8JXBkNzdGE1t5dfiGUBf4mlzbKnK7drcaPq0mpaC17vFGAp%2FWdOFwyKFJfehMvljJIjYDte8KXb0D9bC%2B%2BqGOVG18ZTI3wl0%2BHevE27IrRrriHrYXd0sl1a8vj1KG3IAhkkUdb58px84cdLApY0CQz8fX66ofxUTzP5Qz9rbjcDibq6q0hdQxZEFLpWvt4q7QeIbxMoL1YO25UoSk52GLI46mPGaUuvfqZZ52o24cW4FKRPSTfySZZ1GW%2BQGZDfabCPBmPfwpAuykJxdtheRnHPorWfxRsD%2BN8vxGXSdmOFZ258xGids6uIPReEehTVo7s2eOvzc1E9ZGO8RpVcKVlvh2Nm9YyBsK%2BPkPff3%2BeIbr0P956I4TAHPpPV%2BNq2XfA8iZqKf8X3z6M%2BWoouMdangCVJpIMNGq0sYGOqUBNe%2Fwp7zQJ7EboIAhkZbMUjg9iPyqkPLajy5j8TIOpEkSkKin5Cu5n5caa5wJoSxvW67QzkvcsxcLX2pH9%2BE3lSkper6h352tJHYNm0a%2Fv6yJJNoFxj514TV3Ko%2BMOk7t9dN7GA3mO7uaxq9qcrlWtEJ%2Bv9AIGk%2FgPyFul9SVUdB%2BKEF6TNuuk2QaubNFKlKZQSTo21Oo12JCkFtIAtAJ2Gfs3DEk&X-Amz-Signature=e99e836870476e757503177c1bc6ef6be0d337c9d7121f0374df40455f994fab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

