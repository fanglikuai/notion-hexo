---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EZRDGAO%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T130042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH9TeMLFtUL0wb9o1Rr2jK7jhqnKj00i7X7fVzQitBisAiBbaysljLw3zi711uac73HrBsqECXSaxyDwiEsMRCKRTyr%2FAwh0EAAaDDYzNzQyMzE4MzgwNSIMz21BkyV06jr24tcmKtwDXARca1I8lCwSq01Wip82jup79xmAWimnd1FEuoBTB0NvvjpzyuSg8w%2BJN3vYRHZhgB7JkAikp5cEGFg5mcQi4lxRxCCKi1wODRQjXl3VHO39QHpMbfN1sArmLrkiULh4lZ1OAvJFwpwt%2BcWqijziFe6sGtLECNz2FSXtZ82qFrEIRsoh89Bf16rKF%2FrOB25SU8aCv1U%2FZZp4CSWV5UWHzDklw7nlK2%2FEqBekCVuf6y9DlnEBEHFBs6%2BMAPb8%2B0ZJxoZmSQVm%2Bof9Tr1vX%2BP2qLDFFk%2BqWAAhRZNGSP2HEBAlqicPWAdic%2F7KO7dkUjFwevZsxKv7unQ%2Fu5dGUxJxPiRIDUESQldAUt8Ef32llnDXKm6LQR7MD%2FGQ47hgU57G4nzQMHEfwXtAUZEYPTs6FdSJ8Pr9PiR36s%2F%2BsAKhTpO2lIwPJ3GTdBQhCWa0ZgXsqgi%2FKD9gawREeMYc6UcQFY56mSPm%2B%2FVqHpkUs4pAZPxDS5CNnK6DxzZv6GtD0zz1c1kd5fiTbZs%2BInu6D9tYTXYo27jw%2BCIm8HDodYxNdIdtUftvtUxE%2Bsr70Hkhf06LpV09Z6zCxiDXrdJBzb2IrLQ96OfRWWXjSVKBfguv%2FWIn8ciXBWYjet6LmWYwvqKJxwY6pgEOeyg5q6g1XyXl7sf%2FK7K6hLhMVV9FkMBScbALZa4F70I20rY6GloyV%2Fqp3iS0kFpJyCU5DZGStweZblaKQ9cEB8BnOT9eRg%2FyRR34c%2F8Bqju6LReXZRiCspW7RXjw%2BHwxZsjMsfazL6omZgigGmpyeOIMWy2Qc5pJ6%2BixHUg8Dk39t7fevoYktgAugXpXiIDZpIFHH7S6HFoaNuGq9G1vkKwGjHrI&X-Amz-Signature=0be4644b5477f868ffd08f54fd137c70014d84e7afec04edba70182ac384ecff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

