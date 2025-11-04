---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q5FBMHNQ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T180044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCqC%2B908QnUde%2B%2B6UUEd4zWUD0Yj4Px%2Fx3Hoi3Yt2gpIAIhAOiHnAwiaueknly52ZUSXTdMLzrj1eL9sN2ExUIfp9pMKv8DCHoQABoMNjM3NDIzMTgzODA1IgxMZhHmLUBCogB4%2BFkq3APNirZiWjyVInFM9hYS%2Fn3J%2Fb%2Blwt0SeazCugndICIZwWVw0CmZVQp3GdVsUpQ2Mpsb2Q7nJjcMPi%2BxBGurTKjfoZvkxImsjwCw9FsnVDYaq3FzkanLqw04INaQuj8KqRspm5aB3NiN2Q7BXH2q2%2Bw3Oxfq0SZ2Eh%2BQxlVj8MLDpJ8UW1K8tGzxMDgSDZWfVNs%2F2%2BuP%2F5sZGv3oVoonisWEXNLfLqvmOK5rLbQdHXygNLuIoeAahTNyXAN29MeRH106Kshdoxnd14WFBchM9y9Szn9CobyiIuLUdaYc%2FRRTHEed8l57F8cbZayvyEDfgTuzLKutXQwhT7yzVbDLrwHWVvxaqQbdoIkqX1VCWXITZLbkpEq9jYKTOChuL0B2zIZznfKjKn6ggMytsIy4ZvNig%2B5LVx8XOmDyHwb7Nav6N%2BjD4dwwGBVgDhF47To9UkAZlj0Xu8vP%2FqShv6FziP5wDhqV9ZzmtvjF3Vpso%2Bsn7B%2Fe0PQyPCEvCNZbEudnMREMmC6WCOfq9WiPc2tBtdx990rDtuTMCc3pwGpnyGogYhawB9XpHGkFcfXeMbQ0kvLbeUCsvjSigGf72bKRTi5STHkyCTun2InMw5MNPGMix%2Bae9ELhOTSFYR1DdzC%2B36jIBjqkAcTAqjiCxGaivj%2FQNvWFcLEHOgbj%2BxYf6NnciFRcKishn8jmY33wMsFz1UncEkLCYYRZp3fakG0Uel2pKLSrV4exdUVscUaVRLLpiQSrPah4NdPJG94RUFOxPO5EplRIgsmT8uHYN2JMsHVCJ0tkKs1BE4zm1MF2%2Bl1f0wgTFWljNy0%2FGZW5vT4pvCqRHlVOzDXryRU8qidy4zDWiLM%2FA4cK%2B3hI&X-Amz-Signature=223cf92b0b9b94ee699abb6ac7cae9748a4068f6a4d3bf499aee080da7867366&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

