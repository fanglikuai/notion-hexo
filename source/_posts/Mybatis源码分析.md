---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCPXTPB2%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T020044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD%2Bf6ufiP03%2B%2FCTZM%2F01lW%2BtkV4a3Kezj0gBefgdlR%2FhwIgc%2FKYfZr9fuwSEx10h3o3x3TRBRvOdboU80IvzKd2d%2B0q%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDJSn0ZO71%2Brvlj6rPircA3P2FmGl0XHyPYZFDlxlxt5LjmTpZqSfAq0FtatX3PfT8QiOifNoLCu9Bbb4qECMMRur11ZBBtRDLcj5UJrFZ0%2Be%2BuMZzOSotwqD85uRXtIkbG3E0PVSC9LtZjDqH6Xyxxiu5b%2B2xPnfIGKsNlb3WP9VOhQKsk7hd0FrG1LXKwTCuUBOujkRJTKJ4SWekMVWlug5Wf55dLQgUKtNC%2F7nujBMfvpoNEJvIBDrTGZmsyCONtB5d3Cnj3YEft0vJTkqCwx28x%2BDwycd7ZGbUUVoRdUI8feM%2BU7xtc26nzTIrRdhdEYgSSgeqZBAXJypNUn3Eawm51dsvytGVi6V%2FJce48oiJlM5OrLtPBOoePqS6X6rc6UMSdiWvHR89PsL078enESEuoigh0aLPii1lSYP8SAQVQhigkoFEEMfnmC3YODMzHZheHzY6vsa5%2FoR4729%2FyBZ2QtRzP9fy%2FQtkRk%2BiRl1xzjK0HFqig3MFwwlw27FIoD6OnQSHw6GoouYS6y85GRgg5jF%2FpJ4C7CmUWO1DydWzNCed3MiFvy1PotOuv2OrdGh1cYxHtmsU1EZOX2VeZDG60Es2G7H0K2inW7bllkCs0V%2BS%2FClQ39d8oOku7pD7AlW%2FA12uT637EA%2BMKKc98YGOqUBKWNo2NezRcd7x%2F3l29kLk5IeBz%2B9sqgil9mdHg7lWaYblE6SuorRfirC9cnLsCUp%2FO15cGALZ2G53k4dnToQm%2FL2Y1%2FjKZ%2BoqJKuGCdqlBN3%2BiL5kyWOdPE9aoTF0gExP1nghPmz0b6cyWI6m6rt0VBY6iWn%2BWUQLTEqY%2FEH9Oddva8nPFuNiHh1v7nb%2F4SQwPdq2v%2Bvbgy%2FfNlTvGRuDH3xITlT&X-Amz-Signature=2828c035ad7c7140161efeba264fc49f41ea48046073bf68907e92fda863c8ef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

