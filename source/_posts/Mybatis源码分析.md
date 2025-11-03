---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NA3RVHL%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T150116Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAd3uPSxm5%2BlBMKWA31XwcuEDOjeWuB7B7JuqsFsGlc4AiBNrXiasKHnEBfoqtwErswxTBeltLA8g%2FFKErJ3Y8n41Sr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMwR3Dydf%2Bu6G3d3kkKtwDb78lFoXRfJ86I6ibn8HL2UczAAfgh5UZj2ZazXcCc17LF5lEnvApYvuxsHTO4w%2FjpXCcN1Evm9kSF29eq%2FTQ0HOuYIzqu8urIPRuIjlSUzLcSeErXS%2BSsiNYIduE5KgL0RlVWGxnrm2CXSQzRCaVFOuh2LjiH%2FJLeRLTPhWk3mFC2aIjVLADLOlr2u9HBZLCy%2BK6UX5iH%2F%2Bkp6phXvwNIUW6eu8Tn%2BfNE7xbRulW62nrgO27x4YA5UOf5U6dwFf0LwR5%2BkZ5GVzKJlod%2BC%2FAciX%2F0FeGgublAC%2FayG2lFZ%2Fzd0QU%2FnFCMHXtUo%2Fhr6%2BimBAJJa3Wp9J4EAoGOlPHgCnrbI1SWZ5rshEmG0Mqm38k0YQcLbc8zki6HtUEpUS4K1DokwR8FQROAHBgDbctlHbLKGEC1kaaVslPVoJfWoV2yngIkUj26iqeMwaRymWcnwi14hOigG3u3nDErei8HdiFlU6lwCV0pj%2B9JWkbylJ4b7c%2FEXS5gLzD72sFf7Bypyfbw6rHV5%2F2aZoSBjCh8D5pUTizmxYb8MOMjih9bHDQiT36OPxq%2Fk5zZI%2B%2B5fp3J3OPxe2fbttbJp10xP63gtLJPh8jpUgh1zI6eBorZghVkB%2Fymq5ZP0s8I6owlu%2BiyAY6pgGd5PmoXdBBUproJOvOV%2FK80lvwNllebuD%2BynO1Y3xo7N0S%2Fdx41iEhDXM0oIcfxK8BbnquD7wOfKnrZV%2FaRyvebjTXb%2FpD1BBswx2iGCB%2Bf4q7fS%2BeISuwjO%2F%2FfiR8pZgRG6Tu8FsaZuZ%2BCQacr8ARtTvscS9yrYUtB18oX9FKvaUnbyRDN5tbnVF0U%2BiI5sJDQyXo9HcweZ0PjROeWSBKirIai9dk&X-Amz-Signature=772acaa33aa945033f5c81c7489193341eef6d1b3f4e77a329b6364f3e43102f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

