---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TVZS26XH%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T080105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNmdvE8%2BUY%2FU4Aminu%2BWpdgb%2FOw%2BZ5bVKcwwLZFXSyqAiEAkp59CJGq9qpvaPnqKp4D6Goh0ltMipImsV5UmAw%2BaEAq%2FwMIWBAAGgw2Mzc0MjMxODM4MDUiDCCQDJWo8jmtsmNrYircA3YEqbUBNQ9VlBM0IFEEO%2BbxK6AdZG%2BOszzNAJPnbQX12bULP2spBp291PFwdu4UsRIopkeTfAnLzQyT4KWTDNeVtx3MhuIpGL72kY2BAVtsfXSdrNjpZnVD0DitrDOCy1MfSlTbi8Nd%2FT5i%2Ba7JMVzKuJtOihrzW1xmjRjb1ANBheCMGQL1HyALqEG0ezuE8GJEHMSo6AxpiCqndq%2Ff3%2FTzrvgQ%2FnH6j3D0zLDC4TBvje6z6XC%2F5BToskA%2BhaCPj6eY8LvQMZyXONibSqXBK7z3AF80tvfmTzyMxjJngYJwyz4MUhzpCx0xYT2kn%2BWZjWBTAHoyn7YLD7Jz7V2xTREyYHntuUcb5KqocJ8RKGeIvSQc2IWPKSosKx0A18KxI11GClPOzE%2BCXAL5j3Mgyo3YB%2F7gPgYBa1%2BqcmexXJeqZGdJrvUZrycdwMh47aE6zpN4%2BX5aPqIyco2c8rHGbnyqpKcoVOZ5bhN4MPPeukou0DGhdduA5jQRak%2F29DgX4JOOcA5I3d2NeQMI03PNWSEEOJUPDGSkG6x5%2BrapldcoFg%2F%2B23orTg7SUA0h4sNVtyRxXmSBi1quCZl7lfkle%2BLgUUJW2TH3opWUAaEW9NqGId2caDuxGv%2F89%2BocMMq1zsYGOqUBfH8NGXmS1TPQ4h%2BfQZesEhOssf17WC4WD8NbxWijhZFVCwwzlpa3l853xCqC12kdjqgqhYHxQ7azhr2Ou8apbMsIYh3XxT%2FZpXQ1t8y36DsMQg4cU9AmQq0SJeUlspEV%2FyPzx92HJem%2B5azQcAP%2BklsrdxRQT7RCGIOjZWzZlIBkzyZeIPhxuj6XwDWMsfX85yp%2FKAY6JZNqot%2BMlxV1gBjQ%2Bupw&X-Amz-Signature=39079cbf128509270859c4c51ce99dd2c03ae5f4e69cb281afec2e629a6199a7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

