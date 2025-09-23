---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667GTODG5D%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCpVZgx2XxlX%2B08fHEhlk83medYXiVdQ%2FWQ2OTYjFtAzQIhAJ3K4wecIpkxi%2FAkTTosonoUPfYSg3jBQKhNXUG%2Fb%2FWdKv8DCE4QABoMNjM3NDIzMTgzODA1IgzGZ8fz8PFVJD9DyuEq3ANV1vvBRSbpx8nJTcwiCwiGTdu2ssYKIGY1ghOk5hOzhyDkG212mJd7PaHNMSnYTm9A9lcDejmXU5VPm3P5TVmOpKd%2Bs9kI%2B7cePyWjCgZBdaOdsQShJCVbNdsTWSpwmS3QE%2FR%2BGo16KOM1ychzMudj12jqAT1YGWwa%2FR%2Ftgxned7XIVpo5qeb84QXegmlnfu1lzZhIuNQrDkBcgoCQIwha3yWe6WKtUF8e%2BR3A33CKK%2BmC4cqNSWunEBM5fwtO%2F1elTeQaqWH1PnXpFenxitiFg7Spw1LfO7Mxnf%2BpGopoWwj6zGvh5%2F48SPCJUE6J%2BrmUfZ4Aq5C%2B9FrP4GdfcWuYXumS3BCkROOphOYj1ch%2FxNjT62fzDVDO89oxzrzU9GBEJTAGXktLWamrltmiUD8T9nD7nenxzOV74GzjHjVQC8Xx7gmVMlpfc1mt5iR53XsNYhEPuLfctEi%2FVkilvuMh%2BDIR2arjf1phthGYoUygW%2Bnbr4g4fWUAZybbcW%2Fs7WdNJuaQuxg8HOqCoQU3L5yZacpEIJw6iAhqSShgfdvAJP%2BAId4CBqfZXCCs2H7PsGHd0rrrDg%2FYX5SLotTU3ziOv%2Ff8Xv%2FAPnN40GM1u3Pg6qn5DMCh%2Bm6POjeM6TCNnszGBjqkAck5VSy3rFoju4l%2BMPqk6At42%2Fr%2BsHQkZRqyTWQcm0UYwvh21Wf9rN4Oi%2Bflftjbkg1fQY%2BDLKJrH7tk443%2FTH3y4HJ9mk6jrgmp6AEPxRYXYqyRrTDInZGp%2B396uQs%2FHMkcDtmIAvJex5qPMudmNzsodrDqBcvCRa6v4kqch6dXWOqoInF2%2BvzQ%2F1UQcupyZ%2Bl8BIkQkBqUzhsyEKFue9IbiWR9&X-Amz-Signature=0f365e2729bd8a5f4e1837fc20c0c1c499da7bdf2fdee279fd8f6f9a5384a39d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

