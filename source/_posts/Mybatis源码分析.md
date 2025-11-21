---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665X5ZEFOW%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJGMEQCIH1UeyrFZmrMBHpOn3lH%2Fciu%2FDLRR%2BdbbfTDXsvlu2zpAiA%2F1ESldAtDnXl1EE9bvJ3Ib89pJSM0rU%2BWKHiB1xslsSr%2FAwgVEAAaDDYzNzQyMzE4MzgwNSIMusTc1aw%2BI4ld7HrbKtwDuSqo2ijk5NVRa3J0s6COYL0CuUlYH63q%2BCGlTsWKnoyRqFfMF1Eg5cr9yXTieGcVk5CzktG3tHD3X6%2FKZV1E4Ed2mC%2F9nlEyXvrL4QcFBagQzhmIiw1KUbYgV%2F%2Bp7HQAuzck2oazg1tuYAiqXI59GDK3zL1iHIS1vNioio2V%2FDRvJIXRyYGDKeDHjsMJwUzjtEX0ktMsgixbZZ5Slurop9vdosuoGx7mngzHT30ASzkUfERonJspEkQuj6ZScEcGVCCsnxArQk2UUcD%2F6vBCJnLfwx37Ca36pcVBZuxksC4%2Ba%2FFJSJ4NWqo%2ByWaffml7ZfdHfAPnIF1x1AfBwcZAA2neKMfjuMU10TQQC4WoZ11gTsfd3NXT6I5xstSKEUxuhM8G23NQW4Ot1tgBpY9xtAUv1A5EBnq8gvTzhAuJ32KCiS3eW%2FVbrMGmEChAte7wmRYFP3cgQagjrbXTtRTeU71gAbCml4RfITLtZGmzXpsg9meADsvDgUbu2JMmjnMVu23kpR9TQ0Kg0X3nXAud9hMhKxcCyewHKw9ZV1ttOLxZxD44%2Fh63NCLTQpewCfo%2FHBKNYLdMrAR4L3z80sNoY5VgIWmfi5gfazWxH5n5%2FOohJiuMOQPk%2F5WfhhkwuIiDyQY6pgHY%2B6RITdnKl%2FSqZTxatNCXHcgOl0qIuKZqdxRarO0Uo9NQs7uqeMmxmEM3M2JEC%2FqMLNkvQgYLSCMRYS0MPhGHv82Sk6Gw2rkVV7%2Fep%2B%2BRePlokra0Tfi0shJY84jdVINrqM%2BnCte1t6v8h%2FxMKWDNi%2FyBjfvV9RCB8zb1LCwUjVxDRaislDoPKIizdSUq%2BWR%2BJRdQc11N65LEUymXtVOkI%2BgCxxJn&X-Amz-Signature=aa32baee8bc9ede935e50fce583e35684363aeb1e452254ec518520c811f8856&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

