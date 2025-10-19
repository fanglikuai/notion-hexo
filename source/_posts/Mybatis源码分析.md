---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z5S4JIEG%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECwaCXVzLXdlc3QtMiJGMEQCIDWJ5JOZQjSvJhp%2BIIE%2FRygywqImWvUlrXmh1YEIbDQTAiBJULsr844QsalM2jhm2lF4%2F9xKfBINcVq%2F%2FEcu3uGSbiqIBAjV%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMxytuFaddy%2F4K9SwzKtwDKT%2BSB42PDZgTbbysfq1BS4zFCh1ECoOgcyo%2BAAMPuM4qdx1e%2Fr5jY6BH%2BVj5BbtsETnbe6%2BrEqemnkOLFdxwGqBh7c8HXlYcQQJZnuvlkSC%2BtGS1VhZuCSVBx4k5cw0z5FDgGHihIOXNHxK1E2x0cuu82XviCysqwKg38gY%2F2a03lvWfEu3mE2oRd33rPvv431oC1XTpzxUP837%2ByVI9OqYzqc6UZeZaG5dmeTC3UNcRwjxkJD38R5Cq6yy9H%2FM6q04vOGub64twewBcQby%2Fd6T4LS26uEmOJKwVUp07xsSPMY5tlQM4%2F%2BaWqgYZti9qsrdZwCctB9%2F1%2FuiG2irRmysbE%2BRoAnREFZf6Dvvr3ogBdAqrbed1D%2FdGclF4PwNNNnjxsGRvVg6N61ldfOZtFVL%2FUb9WHpMhjitQVBsnNXXYSTDlDk6SuPZ3HA%2BDu1zQAjCIjHw2%2FeWRZegH7X%2BYOrsi3ZBaQ%2BpjpKI3QkqSW%2Bsv0DG58yGz4wo5HCA3jf9vlFWc5FVUbtasXVrrGzCk107KyqTyizY9pYThyFPYAt99SeUZQD8dYQt%2B7cudjXsGQdHInzvWuXs9rh2n6c2fw2ULL6H2VNzsdp%2FVefXXy%2Bv8hfVMcVRehUPfVNEwxabTxwY6pgFJZ4qSyFfSqRA8pN98OVXEMDY57xD0%2FL4klh35NCsuS6VtB9RlmRRAsVAvl5A6EDglkiNF0stjvGOcDJGjn%2FrxAiYT7OM4wUli5pIx8uC5Ing5hVjpwA8XeIh43OFMW2sjXz1EPJITgIjKOouUUGL27L22jfyDDfX4bRx%2FWgqJQajJI55wjXfGodIZCljD3q0SF3pblyFsGB1bI2Yg8wbDH9YMhyYH&X-Amz-Signature=b4ed74152e1ecf570722cfd08598465351424cb88beca42e0695e7e3a4fabc5e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

