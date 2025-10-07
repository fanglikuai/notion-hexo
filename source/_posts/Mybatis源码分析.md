---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663LTF74DR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJGMEQCIHDv3Z7CapZK0%2BYKqROdOvrfztqTNGTNBIVBE11%2BXvQzAiAwwleujgFvn43bcEPiWl7gtgJ%2BONsvy5TX33Jb%2F6VUByqIBAim%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMcxDttIf5REqIe6MAKtwDWd2Mb7TJj%2F3osAlxZSLRDQI%2BJO4RK4H1qVqpR36rIDwMrCspX0pn%2FhmvvHtUa3CFQxibj%2FOOsotaHXR291bWA5RDncWkJp%2B4Ahm81rL2OXT2wFa8cjbUy5Zpa94PNkByv4KEj9UBE66cejIhRwla28LgM71BtLSFnKLN6JUAVByQd2AFCJntzcUv37O%2F0Zudpm4YCJ5pAtpkN0Auts81NrMS%2Bbxpek%2Bul1rwRPpm1BV7Xp8A42BI3iHm%2BWRE2XhE8h56CAh1TzEc3f64YsnVOlwyqqbzvPYSRXBSJK7mfx1%2FKQ44x8YqFI7c0ILIjwrnQXp37dMyMploPG3XKWWULR%2Fe0wvnrdLxzREdiShpwIPXpCfFhPERIz%2BL6v7KSorLE3t8baqVgIuZ4xTnIBiVp7zVuhBDiQqsrDkHmrbQDrr7nOQ8HK0PEXTUSOjJ0FCkeBPtw4kHehXCRqkON2GgFUb%2FzmKtMtd%2B%2BNRHeliuTolNwgwpifGJJEsaS6REGuAcRwBUGg6JaKgGyMIliKKzkiMvQDD4WStNEPSVWqt6jL5fPFZQiy8lzPvYN3%2B%2BMnVwDXbMiSA87y%2FrPs0j7XGOtP7EDPCaOdqoog2QSVhdDVzQeHLcNcDrtVO8JogwqpuUxwY6pgF5u8hXLeau2liy4clenWLf%2B05YDksHqz6iCW6az8tqtJYlGNssutf9RepT%2Fz4W%2BmZxzLUm26IROdD%2BH1wYSMCtcM8hXS5aSVXjYjbQLok3HXZhtwdZOsU8Y9AVlGN0ygS3zKumEOUNE%2BrvWWMV%2FKMrXM6CVKCYCmU7hkg7E7ysg720fgu66f%2FLhjJRb60VD0iWheBIWiVknJrRK0grGZwiQ2HELxlZ&X-Amz-Signature=d5563fd65e1fb7057b4d0bb0cad868648e8d1ff3647e3ca3e054a8ca83f8f54b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

