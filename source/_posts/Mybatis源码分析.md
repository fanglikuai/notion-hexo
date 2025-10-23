---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGSQNVPA%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T220047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHjxG6O%2BGuOoxaQy6rqN1VvbDjjkZu%2B6y2tityTQfaPrAiEA3NQdVgNEiUHOUsxRB62N7BVnAKCvkwnWVMp%2FTUXlYFIq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDOlMsv2V7rmYcWyQXSrcAzEycMsDsjeaWEqoiwOCYqfTefdoYMV%2BIjFoMqWSTjooTPWXm7l%2F%2FpQTCkv4d%2FrCt%2FpJ4dNcEsZtXAM%2BMEY4tIwniJsJugWZD8niXAwwDULd%2BJEwJ8SE0jrxGCSfEYGhWvMsa9ANFopW5WZ426zlicm9u3VWeVdlDFG7y3nCYckB8EoQA%2F6TmvdxFb2Y%2BKfdtOWj1%2BwmqZtz%2FiXi%2FEWRzIqUTErxUV9uhn0LKegyHov8C4moIIlyzuiIRiBL1GqKuf85qBdemYbxZaGhaHgsIFKxIeqNWAV7vBy1JedfhQGd9vWViCH%2FZRzNDPP8RvH8gs4MCwupS%2Fu8zO%2Bl5cs1veghgb%2FULlLqkwYZPkHmuKkiTOWu3DR%2BDaTM9WOTyzlocfkNaBg13mADonYgVa8gykx6XKHMGqcoRrQgZRluQ6B0fg9AqGNURPiO4lh0ksg41wMTl2zgH%2F1D1ovsMtbkvWGTOj5QbVPUVFk6kErI%2BQUfjBlF14xFmTWprIrklqIOJRiZkE4paQ8f4vgCRuQ8TKsjHItYqViZwIlu1NMc7sbdRqEolyZTf9pFEIzxph9q7CARlpg58I5zteq3s%2BAzkSrKwyTdtRfQi4Q9GnEOhVjVtl43zvYedTWYgK%2BZMPXI6scGOqUBz5CF1lcvgQmQaHCN5dcs%2FiR6cuEJa5hxkY3yLbYPL9vwfrZTpoZNcJunsyXaDAhK3TXUU4lmaXTLrDExJYmM5G3v7EdppxRZM4QmNOS%2FjwvY7TgeKp2gjaITgS9UsQLvMSi%2F6DiMz%2BNDizDwdgGlVdt9q4DWNFUi55YOvm8Gr1oV8kddmvEEYs%2F6uNBGnqWB9gZinldBE5mp%2FFiq%2BuvaJleq6lm9&X-Amz-Signature=7004f77d4a4c5635f77556d0f58246310f99f11324dc94b3bcb84df03b3705cf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

