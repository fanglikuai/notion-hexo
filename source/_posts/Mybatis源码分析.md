---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667P7VTRME%2F20250925%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250925T210045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDs%2BTVOZ0Gl6HG5IZYZptjBYkcs1a20q7E4xA3iBViS6QIgLfpOlM3pWFVXMXVHFuCBH85KWyhRBp9a3QMQ58QlKfAq%2FwMIfhAAGgw2Mzc0MjMxODM4MDUiDEBP4OCnt6qYYY0MgyrcAznzrthM3UHfk3khL6qrdJODhvMvXDGMgkw1BAATWDm2huxauDo6%2BnlpZXiAq174MDeNap6s2Pmf6Eg1ziMwH5FIUH1sdH3hjkGzUXbgnhPxFWMIvB50uSI7Gtu1stLJShalDyKfgllxi3shOIp7KTLTwbX42TaJNGy6gKjJzkeNwK80k29JmdVfzj%2F1d5echw1gdtxNum7FsvQlT3PVHvtPKHfHvIpZbFGkIiA2ogskRjUENrhz8FXApshtOG%2BeOps8szwRaMFTh%2FEN5lcGCwEniQXasiEri1%2FsTw09tT8ihf8qe%2FDyxt8MzFy376LN8cMKPay68YI9TAzZxR%2FVGkNcCTQfK3zCJhbAgTmUunvSeDhlYkLOsZ1l39Hd63Bv%2BLPaS%2Barl08HODBdQAgqyVj0M0wBLswlCDNhiQ2dC%2FQ6H7DqY5yfn1A3p8fvy5jOjxFV0lY2R2WBacvtmdz6%2FrTDY08IP%2FOpeDcWLwcPB%2FgkTv3XBev7qr7KCJaCM5Wz4x41cD4C5qDdjetrIYiOIXE7WcDAK19m87fU5a%2Bj1sg8xLRW5mqP5TCpowbx5%2Bbs86295Ue1s8ynhqR%2FVGWhRgX4hXvZQ2812cBT2odXWfs2RvwSxQTERpqgY48uMJjY1sYGOqUBcCNJF6VSHix3SXFAHZnPaWd6Z8xvA%2B%2BmD37SStXea3SkUdem9QPvqsTDqSh%2Bz9nKWUmbMRMT%2FWVEp3qOafHUOfJ2znYagnf4jxmX0eYw27AKh8eINg12GZxaMSzLIa4g1tr2gRxirQCRo7YqEUMNGn2uUS1ZXOeyBRzgwmgswh5HTvAAJH0OeW%2FjaSXpIk4AHjWAXm%2FCNtcMjdg3H3in6KdEBqzK&X-Amz-Signature=cfb8e63a9ce85ab035b80a330c1568c13176a9633de01578fac389899867b7ee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

