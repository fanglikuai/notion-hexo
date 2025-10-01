---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WMHKOTDD%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T150050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH8aCXVzLXdlc3QtMiJGMEQCIDDWDHpLZD06skkISv6k%2BlWq3vzKZ6V3xCzjdR%2BmXmC0AiANb3%2Bdgz1mjVy3Gj9lxg72UruoDjFqmnB0ECWE34Edpyr%2FAwgYEAAaDDYzNzQyMzE4MzgwNSIM3lrks6wzOAjhNG7KKtwDkM%2F24IVkGtSWztg1BGv%2FlQawgWTTGZw3%2BhhotrXLkF%2B%2FsK1FeaG6hcFmQNNpyE%2BXRm16P2t04t6Gk9TLmprlrINPHcYjFwv5akZgO8MEIBZKNoANM3PpSbFvY5soAEoSXlCkuQeHLc3XIO9HPeBpe5wGiS%2Flkn39m21XcQ6gnMw1zpYanB3g2h7Ad6YDyRPLKUg5Tvqoor6nSmTUfDWkKc3T3QwqL9Mx3TrOShseBf%2B%2BLapGbYaepN7VjehGcPlihLhB0fshmz%2FifOPsLF6LaXKEf%2FObjvdLMtsXbc5Cst%2FcbhrYmP0eDkzbUYJtvzrryTjElo4yF7LBCQBgsDlru7bqaQVbzmuZH1iDU7J6l0%2FNKOqsxGSlk6vt5Sx0vzpw%2B0d9YDUBMtrDecyA2KZk8GOsU5hEg8t2J3PpIM2z2tlLUlyqUt56m%2BFXe41nfLoxv02Y%2F1j1kfu2aGqdWWSDTbrymCR1opd5n%2B6Qwty9rPiWTb6cEVUlY6Q%2B0OYsjJp9jBXkw9ZKBaXmxU2S9wVmjwSVRst9tbtT3J4htQPx34Z7ESCwcxxvBhhzmIH5%2FLyC%2FVVTMsPpeo2%2F7%2Fx9l6QiU2xiSWKAyq3IjMIo8aVNngJEPGbYeburMFZI85sw9%2F%2F0xgY6pgEnjxiVyukB4Lsuc12tZRNpIIC9ZASUe4CaNid%2BclOkBoqfLM%2F4CoEs3whGd3%2BqBR%2Ftx635AdVpmH2kxoQ6QXR2%2F53gsbK98FybrOgHeD%2BRB7IWhVrwGyABZ1KoF4%2BbNjwK5O%2FZw5bdzyZ2HybXnLB6M6EqVYrF62M5LiebquHfsD9YGjFGaib69Ul2BMZSSWPm9G16W%2BuNhCsxn57KBThE4R%2B9c%2Fk1&X-Amz-Signature=f2b58cef0d2305be399f8a43dd0f4558b7f306ad3adcfd799e29a729bb862ef6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

