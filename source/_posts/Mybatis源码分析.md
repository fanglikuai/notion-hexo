---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TF7VKP7R%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIHKjEIQJOzru%2BQYKd%2BAkEoMWa9gqnlyUgZofdsszFBUYAiEA0noZ%2FzQScdM1qPKITCB5lOWEBnMF%2B3Rd5mbNGXfRB6IqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOFT11sRWpfZJ%2BlESrcA%2Blw%2FX0NGJMnWVjPYKVsMWl8gImSJ9X7EV90EF2WYnDtpdxDrz5tI28iAawVP4BraJYV%2BKmtM7lK%2Fc9yxbIovtebMjakkb5iFmk36CyPEsnn3j4I%2By%2Fu0zTafZPFtzoZa0RXASZu%2FvPwZdk7TjTJNCBjxY57rT46WuUh2EwQPzrirAQprjQKYpxSFzXF2fE%2FubYP4l6%2F2PG5yAOYSalZeuhsStj378IpyTX2MnHIrQKjnh%2Ba5qIdeqSNr8GRkX7R%2B9bP7v6bRL0p8BiuYQABfvHowhP1uHh3u32NNNSNNinZjW1X0ZoJiGV%2FLrMEKgll7VOznM3O%2FuzcFY7orSZeM7nCCKSC4wwROHO%2FQuVcPzVu3UQOjwcNVQaw%2FrNeFeZotzodw1HFlpSMV9Ty4WiQ7uMxOlhu6IEnfr0078EV1MZx4qflfGj9eUgiKtJhbKDKNRhCsEo41%2FDfncF5MyyxutMJewttWUagZ6jUUSkuhBCWbAjGG1QCDxy00HoBwsseDl2zAxLTMR%2FHMXDE%2F%2FWzMi8%2BWiPMWsaYqjlil8Z4jrqdpl6kMh%2Bz6FrFtTF6L0NjUsIi%2F2673HTf3LZuU%2FoNqx6HTdsutHbZyyWaxF148rp3breg62RkPbV7gdprMMbIscYGOqUBfqcGKwUlF5G0eUey58X%2FxE6pMOvdwiz7b2fe8giz4QEHfd2M4g%2FMDFyRVsfSuXcMm2QazkYmMDWqDtlCWVdLqcIEE5tAv%2Bk1MFEU8npAixJerLI4IGgKN%2B6EL6jBtWMRUF2Eh3%2F9Hy1auiFXkHv2Rqzal02QOnH2wgnd8wEkZJczJa1I7E7DD3qzD4uG%2BXDIF8cwdHm%2BWfiDR5vLR%2FnlybP7Taq%2F&X-Amz-Signature=888754815034ccca40bb8d37df88e9f063dcc1430f9e180d39d81e76dded685d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

