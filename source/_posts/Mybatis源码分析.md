---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YFEPRFWM%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T130055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHUaCXVzLXdlc3QtMiJIMEYCIQCOLUeVoL3rEdAsrWq81BIffmnQNFRQfWRamrME7kMlTAIhAN2mrzdtgw0g9XmK32W1aLFkt%2F3q3Fnm7A6msracTKsOKv8DCC4QABoMNjM3NDIzMTgzODA1IgzZQ4KvGmp1irPWTJEq3AONfjAtxUu%2BN5J%2FVKJsJ%2Bn2jeX6WGBKBedqR2AXa%2FbpQXH%2FIlLTGSKOewWra%2BZ%2F0RnuGohxg9Uek%2FBbivHIFwTkfRuV9aKLYt%2FHN%2Br%2FHkSXocEcamdA%2FVSNg6RA61rs5UYOA%2FvfdZ5Bb3yUVDeuDwQYapGh6kAXgr3rSXczRJc874XlFDcnT7Zw4Sp09Vt1EHCrGdR3jUljoOVy%2FH%2BYPSJ%2FZLWgbAF%2FDXpMkH7m5tdkh2vIRX41O47wAfYmcPGkPb2SJcifPbu%2FxBOGZjH4sPwVxR6U8H6%2F6oSuJ9UESSNZLJQ2f2v23A1BZ3jU%2BztfDwfBW1HLFaZTjrpnOyq1rBvFMvyi9sQCXCWcomoTtvKDPoEE3BmY8JAWgbO%2FYHUAJ6Y6dHEC69pthzWuaEPG7WD263ppuwcN8raEpYI9tTvLdME9WRqIKq5wA%2BkbPwCLhrESqc4bYLl%2FdM2QuuXC7dLwO4CLuRPRJR%2F2Gy1y321GY%2B88%2FUhjpAbvIGnY5DMRNcrVw12xOv4pX9EW9GvjE%2FEOL%2FZIGO5YDS3v5gUGQx7f1Pjv%2BAzf4XjHm%2FbqQ%2B%2FdZ%2Bs%2BQ6paJFo36YgO8meb3b2re7lR7IRXVTEQxGsAETDeIENSwLCznGYceB2NczCSp%2BPHBjqkAX7wMUaJMJw%2BKjdTxS8%2BT%2FD4W1UOLFjLDrNWdz82S10G%2BuCTVEkrbBqmr14A%2BmgcIs0iJeNn3%2Brdc%2FQwCpkRvIXBHywpnUs5TEiwJ70nWPChLkm5fDzlVrdvivuyZ2Bbhcafgw6TZHKIXMw0XjBfBTqpeNAnYA6gv%2FkeUrRjeH%2BZpDakHo1diHhVOteAuBl0FIcGBetuYbDDa%2FCEyg76as33ZhBT&X-Amz-Signature=17a8c86d7116542d5c416b2ba950c3dfd6b70935e6fe59cb2066626ec0a0d1f9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

