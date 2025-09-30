---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SDTQKI3%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIQCHdryXGkbwOIhBo6NJxCeJaO71nNPdePMFlkF1eOKIewIgLpgnQmP78tjq5RXqwdJviluXUyQJhh2ToKJYOCbS57cqiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCygbbrESgfW4DxOFircAxSAsP6TVCk7LsXNdRqk16l%2BnZy%2BpgRd7ICP099Ey6qnv6YT%2BpwvGJLANuC0bXm86BiraH3eAb%2Fk3lSFF4AraXBlFwCupBBxL8PvKlrWV%2BKT6tPXaCoAJc5SOFOdmOw%2Bk7H5aR8qZDOzayLNluDwg0GcEs34NfcIxxePlzZwzkgcEfY67IeWtTrgKOWPaNduBP7FV%2FaVYZFGD9E05jFYwrtik8FwTMivUP%2FlGji8Rw5ptQ4xN6GJRqthBItrI5OB7BQMqNCBojpbQUBguJY%2FNlrcKcdEn66YqZgmDgJDKMUAxzav%2FHLB02DpEE1zWsk2ZgsQivOTLuzriKol%2B08eWSTsKKAKf2f3pSLSBI27dpnJcLX3C3EkwO5H%2Bu6MpPwwtZ8wafI46aiPkBfeA5mss5CoeObwIVXwMK6QygSSi2kx5dfAMghHViTpmOfGT1PhHivPwErk%2Bkh3PpuUPm9HcWFliQpYuYMFdNzaojp2JuI22p%2BKFAbgDHrBuQlVnXydTuearl%2BObXnXU4YnJD2lYDDa8Z36MJzEaS3jtcNJJl0dH8ndwDB029kxCHJ1qskfwQcHT62kZU7zmvS%2BhKMrC7DG%2FjTkMY38IuB5mddWzJ%2FC%2FDP%2Bw4xGu9%2FaOeXYMJuy78YGOqUBRe1xmFTEIYFp%2BkScGRAIf86rHk6SW9M3GLaeuMedwCjNoqdCuARBKBvFAB66IjITJxE65j922gD%2FdYsgf%2BHG2NlOOiIbP7UwccVmiz0FqzTx4cWz4s7tnTYTUi3BOQGg6xcRXpndk32jA16OUGxJnnI99W7Vj%2BeqxcstngzjfPyAE1ov2XVpeQF4kM%2BME54672QpgKWQnLZGdnud69ZuO3A3B5Ff&X-Amz-Signature=fa1ca89d08bddbec678bd880f4a5f3d5463fb7d86790719de83996dc350db9b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

