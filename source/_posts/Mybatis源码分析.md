---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46667UC6TVD%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T230043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJIMEYCIQDhOecmkRTJTjzzk6Q439dYst26XgnfXiGcx1t19oXbyQIhAIAWcZv0QaqlAYdCoBJAsXF%2BObxgl7rPavbhpKGqQSHRKogECMf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwdLRoHIB9zz9lcQSwq3AMZr57y%2Fi2orLnSox8wmpPygDL0Ea1j5HEGnwRX3aQ0lImoiS23TfdMI6VFEj%2B3DJRZjWn5dtJzhamorrWLLROWOiWPQdAjAou6FrtUNuzX4jb0PnP02FBug%2BHR%2FeO0WWONwpa6gGfKDnDz5xjwKN7kAZvnJxVrKmDnNraV9olO9C%2FlEDpfrNDCFw8D8olvUy33UHSqXbdTYt9h1yU%2BNaEfT%2BXms3PuorTDZF4V6sBXcHXYqSB0B0LjgHGvXzv8fWsIcar59xT%2FhwGqiCbxEyLLtqDbNn0AZFvpaWi2FzuEF%2Fz%2FjUJlRhHt96c%2Fy0LEt77cxfMCFuhx5MkLY8lIQJXSiAZL1yvVen6lNIVrMb0FyzIgi4lt5cZuoB%2BMbKwpvDJVofl1L%2FW9L936DolrQZ%2Fp2sYdqcbeNfswqp5xEjxTUm90pVSpQG0PFSpxF5N9PXE7oS819qZyjbDaO8Ngn7GJtcuBEaqtd1RUjxNA37nAe7rhoq167pCJ6HJs3E48Z7GU%2BfPEwlI3%2B8Z5X3QG7etv97XGiNvOnp8FyyiIfWgdUm5Baaspl21jEMt7r3iFbSQFbeb0XRXS2o14J48KhH3O5X3hYhAHROLprRrKoTO0Ajjwbfwha4cI4Pc09zD%2B2ubGBjqkAWO3Y5mVinV3aaZPwMiB9E%2BNnwcDk15P4Mshs%2FdrEpjSvEr5Q7u%2FPgxHdkUHUnQLb0Imd5c%2B4uUtK3LSnexYssUY3K0p1ovEKB4BNSQ5AZgmPh0AZvJ9x6K7Sweyfmmttj3XnAjDHSIvTSDAcBwmZy50ICYBPQWvz6kAOW99MITOcSryJcg1HOLKLNVSgj5NToBpY7rX2dQfA4Tw9XVM75XFD%2FOV&X-Amz-Signature=8324674f6e357d7b822876c1a39584c9976817569432225878f6ba8a81425dfb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

