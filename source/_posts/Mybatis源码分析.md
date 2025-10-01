---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667DYZ54ZF%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC6HpvpTbC4feVVWBaQp%2F%2FZVfcerMTPrPt2k%2BSj25U1eQIgH7tx4rikaHVsxeOTdn6wGX0KgvcH0bzObxppNaQEBLQq%2FwMIIBAAGgw2Mzc0MjMxODM4MDUiDOTPMBZoYYjkl2z0XircA1VHkX92jL%2FZRk1MgmKgf2sFrI4uF10X88z3smni%2FMW%2BO%2FARH7D7XrLsdoRvPd02oN1QWz5WXLrNMVSEj6Q5MCI%2BIEa%2BHGDZx3WYHQYHRoX5C3dRRx3ejJ8vj9ygDnh9W6cnqEyVBJUUxVGfhw9IeBGTT%2FlGbME%2F8D1hAJ1jGlac4Xb5WvSBjfeUc%2FIlhRAX49vhH7JUSvprj1aLLCFNosIiMffU%2BnO07YaabBQFcnr104Jqe4CPKiXOdUdp2kRjNz8QibhhwrLW75ktioY2xxUgDNbshWvvtdEFnyONIDLXUl1R72K1uRDG4PqmlgdB1s%2BULvaa84K%2Be1cFquAKNpgyH5sUj5U%2Fb4LekbArSjAKtxjgKhajwNz9LvO3trJMockEbV2myuUrrZJEMz%2BkuUzrMFwK6sTw5lov8KDPdvl5Ak6EVlQ%2B2tr%2BAOdeZTol5ykBH694%2FUzVP15wi3Sevv6yVBsVHUXq9ui9WH189Btke2HjLN%2B%2BoLGdZcT9RxRfbLEyfRu%2B275n9WdoHBdQocoGCl%2FjOl7kQEeqneBTz6TxmG1qiuLSUAFH%2FtjQIhRMs5R8qi0odACuX0sWQEOHI7UOvxADewmMfOWXmRXrGSPZAuf7eSUi4%2F8ETTIxMKbV9sYGOqUBdsuhfwU8P8YF7HvO6N%2BtUXhg1Zr7OQtJ543j23hsnSwOkzfbNZR%2FQn2eRrKP0LTPpDYF8%2FDtQYmzMJvRGAWEbbRARvujX%2BfOj%2Fj6BsBeLBm6bXD7Hb2ZUlQbnGhXkDAasW%2BaEUkPELTZOHD5lfdpYVIB%2F8gkLCsfOzp6cjn7lPOJGLlyANdM1pCHKPFFNw1usmVx8SNqpHIv9f%2BkBM0usyvYkF9k&X-Amz-Signature=006cebf05553383fa8fba2ae671532664012ae5e3847879f9965bd339b661681&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

