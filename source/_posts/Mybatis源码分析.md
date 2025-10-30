---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z52N2COV%2F20251030%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251030T050052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC0aCXVzLXdlc3QtMiJHMEUCIAKAUjAmPzbtgwsylJsPKzSwG%2BBEd6Sv3Mjnk1wheW0BAiEAyXQOrnwWWVd282PNfR7%2FUi3CdmBuvYe4ejvUn%2BXkCTQqiAQI5v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAysy5gjNIKpZW%2BRcyrcA5F5199uJn%2Fre8jy7Frsdb32SVBaiHgm4G3ADayfe2AP0YdzZmSMtBwEbLPYo16jgVCTDgpM4qtsjEtu8RVkdtOmuIOh%2Fu0bGRuYPDHwc4lsb0Bl52%2F4OnE43UTeEHT39XcfiAwtcAjnnoXMJyj7wEw6H6LamvM1Ox57uX5s8QwZlfDyktYzEOKVq54Zxjlzc%2BSgVoExSanR25cTE%2BNcHFhiw4RRHWvw70Rmfto1VtZ5XquBSj85z6jzTyR2ZPF9Qh6QvXSSN7bBwDNgFZoUv%2BbLa4nM190%2BmgBgZTVD35xAR%2BThomH0wwHPt01mTwtOarVRJoG1iAU6wKNrhkfBQd03frjqKiU1O6lFpQ2ynERCdsnsuzZJLEExndM8sypIue74c42FlSZEWLFftmtisSc9r9mLt0rJV7Hm7stCWFrdRc0k68YYOKDEfgiITezSkJXy7Y6EJja2ix3FhJvu2hG%2FctHjl84XeijiHHrk7UhOM0U%2BuH7HOuvPry1MNnuh323MKcDmwaHQ%2FYsAsszXUX2UogjMt1J7%2FfChJpZ%2BZ80TJNkJG0UQRenlg1y4KkOPorzB95lKGLH2ObLK5ZC%2FZ8soJ3aCJiBrmieKMecKduoieVGew6Mf%2BL%2FvbgR0MInRi8gGOqUBIUuZTl7g7MZ9JWURWq4dYNbyGr6ff4VqxSM9KIS0tNN3SUtoHuwfwX4fXayXK70RGzwY3hbs%2BjtBlrRDf2vEv4YtmSWnDXxh1ie8wcDUwynA1YNygRNyM5e0GVK%2BIVcw61Nb%2FlEm6Prbx0D3sytRj4Y6KoVaJkNnBrZQvItm%2BALJhN1OPoI2q15Zd6%2FiicPIHnqa94LiiGtt93Noj9IbdX2qbxSa&X-Amz-Signature=9fea9f2db40b6df2e00c81f76a4bbac585b69f38845dfbab2e49cbde955f378e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

