---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UHAY5CDD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIDMZYCBmk%2BSG9TnoKCDYQzaSdQkD1iJqIJRNnPsxBLjzAiEAktKzn3ZsGIx50c5Vdtg540%2FZ8%2FJhLhf5JkMw9SzXlA8qiAQI%2FP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDHU7O9H9HNk3y14CxCrcA3o8jxr%2BqvTv6%2FzKr4nso8dVjRYVjqGKVenjVN39cLFdxoae4%2BHFljWSv9JWYRxp2AAz%2FrTMkCdwRxtscMleEWpGjJIJlSmogeHan8aNsqDHjqx6A8mvAd%2BYh72unhGyQcw9yYLQ5H%2FEazcDp5QwsMPRQRSQt0NblJbvC9EkphLsi0nahCbdE7x3ZIrib6IJn6dwb92WLhaIsSH23MY3U7IryAyavbCjP%2BJYL3n7z%2BLq3eDFcHf5Ij8P5aRgsLYKWuD6kB%2FhmLgpvgkjAW%2F%2BuBIK1i2dJJE7mFA0aNvGat7ODkpJAA68xE30mcaf9Iau7DcVP7lthKPrMtQrileGfUXB7Ym%2FWc29yf8u%2BAKYBoRdf8zCrq4GRxDt%2BT4vzE%2FIxI9k4ug494gVqaZS%2B2kRVPCRsppgS9kpQtYQEtFhG4%2BDeJQfJ15nMoxMB%2BFWYLdAkzaGwAOs5peDtwDHhOIxZ70xviUvtic%2Fps5InoyYxgSShp5lRahIaiIIz5%2BKiymbClQz6uAlw1VwmNikOyfqbeQX%2FYt3LvkVdtzpdN07n807m2OSQvOyTEiPtGi9lbBsgEnC61DEfoCAkIPlOqMdSaBSij1Hxs%2FAHDX%2FeLeIKB2dGhGrmLimVfUVtrtwMLjq28cGOqUBzlfHMEnKqGzxlNxKDeKrTFTCTvaRLLSJxrpckTBDDcQiCFasP72oY%2F7K16rzoOH%2BtywE%2BeK3cN6Jd4n1%2BKaRRu0Mjao6pmGerWsGRIfOH2%2BwthUuRae9aGvRwGlqAkvq3DgmSc10TJCB5HX0YlQC2sbDZEh7JZkZuKDFHXZtmUW0FPgYkfN%2BY8YG8D2W2CkdLJQnOJ5BrUXUDrDTcOyMQ6TWv1HS&X-Amz-Signature=d45c121e7e2ddd81c691f9726883076b18fbe34474adcbdd493ca050f96a99af&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

