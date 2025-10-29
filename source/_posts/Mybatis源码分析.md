---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6EKOZ2C%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T060045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBUaCXVzLXdlc3QtMiJIMEYCIQDN%2BpIGH5Pk7UvCT9jYKqofJNmACizquMDHGWfF7yWoSwIhAIRyWah8Grff2z1Ee1IBgipZZ6Y1NleQ%2F9qKMsHUST2TKogECM7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyli5dEnVRZIcgdO1Qq3ANAuMvIlTEiubYs1YQ0mxmQRRab98mXC1IIx31mTUTjxIRO9DSMSUYkOC%2FKoXq%2FCiL7UcvLOfjohyAiYtHqZkncpFXwCQuf8QnSTz3VcU9eBzsD5ARvoegh8cuKR%2FClhxspcjJLaTpFbNdGKE8zmzqzgW0lFSnH7xE7%2FF2b2smw8Yjn0ZgIu8WolifCAYVyxem%2BsX6uMQISWZRMhqVHgmspEOzCBEWGMDwlTtBUys0braIlIyGiV%2BiGv8zduPbbfx1csAP5bKG9Cn3Nl9i%2BdW9HJrIVo3FtJINxtTocihqZH1XmxDeNYBaFQevPvBhaLWxLylqSqZMrdGsgMlnsqd9IWVKov0GbBU5raXsiLDAOohjwwnc81k0HBZqmqMiLKTpudtRnIkAsK0ZGRLzLlhAuxDf5Rp8Ac%2Fh1%2FZ5yUYtEakD17abcKodfo5nwrxXoiDGVWe9Pjw83c2PJAon3cu5PcEUa2AfuluVnc8chpKmilr3y7%2F9x%2FkCSZCrddxaDxS0yYai6jwITsv0wIMdBgONjiTfeIpMv2fjl%2FapnTGLAx1BAhEr%2BlwWAG52UD53weAZQWGEcnHbpIBXx6nhQAiHFTd%2FTmh%2BlEL4rzmebtML%2BziJ8Yfo8KW%2FMX%2FezrzDNv4bIBjqkAZ0Mtu%2Fu6a0awDisx5EfUFrs7j%2B2k68QnPJ72df6VwBIfcrPbkHswoIoM%2F2c1y%2Bd2yKrmLk%2FeYJC49hbevUW5MJq8GKl6HFDCchIPhVZZqdwFnEt8DmO%2BlyRp5gCyWhoQTq06P1Qbs5KKHQcRmPdtaBNyCRqdmohM1rigTf3wEOpeDZxG93VCh607V4oBYdro9hYV3GZVnWb63gMPu8PRlsrk1JQ&X-Amz-Signature=dcebe96701388ddf36e28a1f5cc4b437ca154f5fe9a6261aff173e443737ab10&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

