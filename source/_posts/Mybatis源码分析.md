---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TKBKQJD5%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH4aCXVzLXdlc3QtMiJHMEUCIQDotwumjP6BDJzxMN6aDH5JLgr9iHZZ68Vc2sT727PlUAIgIEQiV7u%2B8Y363Zl3ZpWtRpeRN4faACylzLhvB1yzqFgq%2FwMIRxAAGgw2Mzc0MjMxODM4MDUiDLb8sGbCVOJdXTUzwircA2iCiqCNgXhmBDwnDrJdr%2Fnz5e0VFbHDqB9%2FB06E2vjwH14hiu3GHQcgVPz7w0UG%2BWG8siwkqMJj6nVy9iONdJNW4J4oRw4%2BNi7R%2BrIto5jv%2FPlUXCzynKI%2BxwdvNbyafwSjWrQ3EeoVst0d%2FGkBiDuHLxJZNzzF4WFm94fg5biNX8oMyUq%2BdZEqS0Z8UFPCHLIcOfQRHj%2Bq1JXT0%2BF9XeiHtf63RHwdWl8c8FpWL8vi5p4Kr25rqkd85JSn3TrbrfkQzLv4YxXsJstBZY7xjBJDC4bGzxGmcGs5vG%2FPiCd1AM1elSzFrdWwbADmvIFWjSNcotmnxxsjvHmnGLYEzTA021kTIawdQCwZyQ8W5Dc7Zv9TyCUZKZIo9cUUtOrKB567E4OjG5n8Fig6YoWhIV06FxkZitKaFb2D95s8pcG2P7%2F3klwt71czvJKTAu6xSmn771yLByoJwwUUxB%2FWphv02ElYlJdiRyz1m3Wjz6WB6ZpBNObBg%2BvoToDRYxlDaVO5qQZX%2FfY1Mz0T7DaWl25KA5xBBumy99uZN6bLV%2FNcIvrgbxGgEA48pBQx1Z3igY0Tn6cusb2eE6u0b9%2Bw6DxJ4FglynaJi%2BOHJmhSnJCSawiw%2FvJVKMWM2xLEMK3m1cgGOqUByGiwLxtkgwvdLENT1dFQOAxo%2FFX4YoQnYRW95sw6yFZPgJJoJP5Z5NHRs0eGyQgfPgWSlGuc1eWHOxBcrAiEfniVAGz7twdA%2BO8marsH%2Fwnjncs69kLCdLnLKz8qOfSqEcRLKywA9CwGrS87vFoQB7G9lGxXXyMSuZP8MCGIyzW3s6LrStKq8DhF8xbyP%2BVzo%2FguzLUznR9lSkz0P8fRK5PI0ORG&X-Amz-Signature=fc95b723f70adf4722c36f4180e79a1c5a54aa7936f259530cf4326b4d0a157d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

