---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCLNUJYM%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T130048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEUaCXVzLXdlc3QtMiJHMEUCIQD0VAze7xofzPAbbvyK%2FS7L%2BJCCaddzzgccEMOI9Gz6WgIgQqtzKQhjPL9TpbPGJ3OqGOVkDzE3xhfmMuWgleVPRLYq%2FwMIDRAAGgw2Mzc0MjMxODM4MDUiDGZ55Y%2F7656ZTASo2yrcA5L15IA7cdv%2F%2FDXXGHxsioyRSo1Oj227178qJmN4SeqqOqvcZZ%2Fv8rGM20qIC6LPstCELdJYNSDpmGV%2B7Gl5kqcBR8lI%2FJR1uABo4lCR1UzdNbpPPICkAEx1Ip5lkBN5BwXmL1q9mDxnbmAVUa8jRtAaMV9IK%2BYMzg8deED5%2BcABxL7oRz%2BqQtknJTEOCUr3xstX%2F%2F%2FSWpS0xypNFPdSiDrrH9%2BsSUJyKWMnscUV%2BpdknNkfFnG%2FXCHafo6VlJBcBN0iBIcKzpF9IuNAynrJyJenvIGK4w54VsapUR7JRJwRGEh%2BLwUD5oP3nDvHUolKhqRvG4BUTkjSbbryqqYt3agatCMSnnNaDeV3S1Qws8xRDSZARyfdmQS1DEyGJ2CECG5RLyYdDLhzixKmDiwFhQfrwuaIW5AhOMXz8nD0IAdduRK696uCRxjRi%2FiVoc8gNFJMZtBgkJWiVJR1MT%2FtS6SiulBrfoFCXQToHBNiCBjQAaQxpfsUH89lnJXbdo9j4mXuqMokW5DoFfyYSV%2FbS%2FDNteRY6DaU4Pg31E5BZJQSAOZwGRd%2BGf45G7BCHuSHEaP2AlyWbnlDMr%2Bp8ECjj%2FXF96jNiOjoUk2CqY5vGpftDvOWWYpK01FvrK3cMO6xgckGOqUBypUzSRS%2Fxh9YS9dBVTAOUPAlkadKL%2FhinJFZ4A6ONYr0taTWIAUDdsjnFkSaKWdTxMMBsQaCGJZHQbpf1S6WFCI82jHgSuaGHKG923LXU26Hi0vu30xkBwY8q%2FCDgcg%2F1ConrZjT2axTOWDUyZZLpwIPntyPiowiYTFuQKH%2FtKVook1lk7t%2BdDy3o4xQL%2FdAmKGzd9MzBf79nlWDafeZSxM4hEUW&X-Amz-Signature=79c1d5b7ad0a50801aea13480a6849774de011968e6e855c7ee1ae0d938ba6f6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

