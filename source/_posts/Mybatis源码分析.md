---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466273ZX6ZX%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T010050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBroSX15VoP2IQLTOL6PzziDO3HQnEwSlbE2lfkdjVpJAiEApUeFZR%2BVuFuSuObxkArreILjWVQNQo4R%2FenH9jSLTWcq%2FwMIahAAGgw2Mzc0MjMxODM4MDUiDE0lTOZ5g5ho5Fwd9yrcA%2FnykHaB6u%2F3%2B%2Bcs3Fyt8yeazukZT8oIU2dmrxJ9Yf3ByuTMu8YGmZV%2BxJgcWIOKFtZ07eV%2BOhspSLdLQ4Bzs8AG7K4VT4kxBYgasF%2Bf%2FLLvKmuoLj2QiNWJBBcoRBIzfr3q4sGQDa3ZeMrNg3iIdlPZuOOlEw7XM%2FRR8YKqcjVJKtkAxBhB46i3QVL8DcLGbIbbMZNmOu6dYgh72MpOl8OY44WCfgmRIVmO1vm%2FgSlNfgthDuJbFWNGU%2B95xg1C5YxxkEEGfN2wbq0RdLiiN6oJZXthemInOu8c1XNbLmXOiilvp9KtnyW12RuU6kIL24PeYEgsiqs0vdnOSEYdtRNXJ%2FRXMRI9%2BF7StPCt4gwVpVZBNPxTcUfckgc1PZrrM6j9dFXS%2BKrQqSPltf8s0%2BFIrUOSdwOg1ncmt%2B5oc9mkI%2Bj577k7%2FTrqP%2BqGF7ENrnRsdTWb0QSedyPxan6TgC5C4vivgEQCTHUSAn0ckfTn5slYtij%2BWG93%2Fpcq1kjxobM%2B08foCoEPSM6zPeyua4Evo3NxFrgMKz96ehRzfcyBx%2FdSGBj%2FQd%2BU30wVzKBcahFLYS6ScDUSxANJahWXmKGUmbt11kfWEp4Vmtg%2B99WaV%2BmXDIA1Q%2FwGvPEdMLmz8McGOqUBu6KCaAjAR%2FCP7ASnU8A0yHI6K2A18YoX5gFZ5Og06KuguSdSu1LmmxKsZSj6oikyBhEqEFfVYIQg0eDqegPTE5q%2BG%2Bbb2YqTWrL7OtuelG4%2BwFrgudWzmsZvnAHI8q7PRpDgRondNlAHpnut766t0TQoC%2FoYulttPaFpH0ut7h%2FXhYZSQSyX507KWtv%2BBESBo9Nnj33DIvPLTOsfnfQsY2gkcKG2&X-Amz-Signature=fa032153dcdb05d21d47c37ad4de8650555d3998be08c3b355deb5de81a546d1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

