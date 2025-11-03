---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SBXJ64YK%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T160058Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIF2V0GURuJV7XZ6jTADIN9QrYSvqJrD5RZvG2Et%2FYJmbAiEAzbJtO9kJYYrSvy9%2F4mkQoE4MBOUzfgbqkpvi2EJMAIsq%2FwMIYBAAGgw2Mzc0MjMxODM4MDUiDLRAfK3zLEX2GUcYvircA9r%2FP3ht4g%2F1P1x619NLZprYucy1a9b0Uz9eYYGZgSeaW1aBdh1BtJKBGfOYyOJVnJ0GettK9cOFmP%2BDN%2Bes70wg3mAdDlKEJhaQbKYVBR%2Be4Qe6G6l%2FZPnhhmH%2BJ0scdm%2BIoPz5C1G2uPbp46LztrS4j0wPaZh2sI%2FU%2BOffNX57VMN25ULvvedMKafqnicBW4XT4H2v2PHiBjN79HK0cu7%2Bb3TYtE3ogtxnAA0YqNhyeE7c1%2F8yKDDS9zXZk%2BXrWcInhMVc1TIYOciGAG3pqN7WqG2ztpuekVm7%2BtfIRjaztZYycRyCQqziKYTGefq3jlgyNGlWGdRSbaeoJSLdAgydLE%2B5QtGaY7UBSoiL9mrl519%2FOCYwrdMREXq5y7fCwkH0o5q8e6wSFIKaPJQmrQ7SHgwTlFENVqh%2BtdoEimh70vj4Gs5edUaVwjkLEDk2nfOHL5z1cSLR1YScNumlb7qbNpmt2xe53y8BC5iE5Y8uAzf2Dxk6Srq6TZiE5DlQSrruzrwKD00ySYtZ7HlTG9aID2EXZDzSJ%2BFC0ivxo0CkiU9KaEbM2FOo%2F3by%2BTecRBHmvxC3kJiyZnn8fdk5W7d0ouYcjP3b5HBWIhZtDzKdsNlAlFpb0pvm1Ju4MJ6Ro8gGOqUBKVEMPDpiRBTvgOz8MTae%2B8xD2h4QB2vY0Y99OBiaIHHCpP4FTWc88m9vuwE6lM%2FYmbYRa1%2B5vYJYPJ%2FlW%2B92asIZ1egik6%2FWkEUxFIt%2B%2F4Qp9%2FmGR0yz13LJeFaWVxh5J6RLggFZZTu30L%2Bu8ARy2uL9MWeO0XQ6SdosQa0ufKR6eLVCoGjZtiatHKUHukSoW5gXBGYT4UDfRzaPntesDB7GwuhV&X-Amz-Signature=96e52004e6dd25d49a9dff48844322b90f671cb6ceafe18c7bfd1a44683ea3db&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

