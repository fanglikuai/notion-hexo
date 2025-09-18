---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SN7ZQ2B6%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T100051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDRk%2BlIpVD7APlqCrx7S00fcaRlptvn2WMan8GpCd6PhgIhAOVtzRorLR5X%2FAunavVdBKAfZmBbNO3Geo5zgMyHD8bGKogECLf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxFFxn82MyYgDqmdZMq3AOitu0wIaW1OlrsdWqj04%2Fe9RY1ed3%2BQkEA1rFrZb61lGNbrQukOXeEy%2BryNFcjEASjCJWgHqmLwxL1N3Z4H8UGxqTAsE3YMGB5kKall44k7IK0X62xR2SItV7sNfIBnY8rEAMzaUmLkXmbyq6MhJ3lSnQxrJaGjP5slF6nHADOjOtK9vSa0X81YM5hO7s6Yg2OLxc3HJsKPVoYlJ63yJZaCcYT%2BADBcA05v3jWb%2BV%2BlPQyazCtkSiN%2Fm5%2FmcAoijYlhRL8z5vQsuwx0AryhC%2F%2BgFH6yyjlrbN7Cebdavq5RF3MlJ6JeuFwdtbD6DAgITwni5edZBWZhGlZyA2PiYwqyPrhhW5Hx%2BCXbEP%2FJfDtbPzOUK9t9nJMw1uPEUPjnUfe%2B1RoK%2FovcjLSv0uxBP%2FVlefFoAVmZZGdIekMSSVRu%2FziYIye%2FBhJnN8kAcra5PDHF43F5As5WBYaRzei%2FiIf5nAqSId0WAgsAVtxkaI7hgJAZqr5XtiGtgIjJ2quSiuUZILhTlVvzw9xYXRuW5rF2WwuypOjOMhiUNgrgjbHTyb3DaO8LdlIFZHeIVfbtbt6fW0f4p3maVZ24j3U%2BC0sYOYADM4sHSXpTLzSnI1%2BBtyuBcSbYaQDEwgDvjDkuq7GBjqkAR0PUaLRg1PVQFBFkM3McsFVbh9OldnGZoY4le4KhisjjTLG%2F9hhW525nqvkzX5FmtU64HDnlR39TD7q%2FxU%2BUfbETSF%2FBJLNV%2BqN1RF7%2B5POGNpiNogbrqQPdTOnEUJ4ezYvOvECGChwGugGcIzQu6exwG59Gbk19RwC8vZOljOaPAfe1SF4PTDW9LEvNAnwWt2K%2F8vRk05ZrscqeR23l%2Fwo%2B%2FDU&X-Amz-Signature=e5cca5e36946e0eb5930b8445a6c1e693f93f561c230474da34cc47a7dd7afec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

