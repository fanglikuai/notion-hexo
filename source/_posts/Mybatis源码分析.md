---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDOPS4TS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIDxIa%2F%2FIrZp2xRHTZK3F8gEjQzGhhornFLvQJRQPkj0bAiEAljx29AEEjCcUmw5L3MxiofZKUUx2VfWapPi2GE03eiQqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIilQjwNsMfdNcx1NircA2YI0tIOhPrx7CDRGDKuzquzhnW0Ahvcxbnh3CU9aVLAr%2Btq5qTliLGe9gJpYfH67ai%2F27YC6V2IiW3UmvYn2pCvexNQYHrdPy2bApO2qZvabHQq6sdWvZsn1dKX7Pb4LLcK9tFZcIv2w42SHucLfGOPRlTyujtC0FB4XrmVyTwV5ImXHUf5KE068I5k%2BsbZGNUBgO0vJi2HVirs4Q8%2BMsThGEHMKxCclb2IRmZqlAl6Z3O5GbLXodmaTL%2BdMHKhhV6b%2FhqaR2alTHE%2BDgp6yx6zD3J%2FzVxRVx2rdmftLDsNG5lc5DKuN8PWeCsid0iFfaBwh6ihAjtN3h5W0BxDaVzYbeaWkLjhOJEavS9d%2FuiBKt9F1EY9FMBPEjJtXofIDUk%2BYtKBM%2Ft95byKs273u76GDsisZayZM5sJnse8bnvXboKqSY2Ov8KS0gKuhyAOOJZXkL2AFInpObDhTlYHXalwD5IJNWr1DADVtLXA9hkyvEh4mIhdS5dSYy8w4FyRu2%2BCGKaBtej086z%2BHwEjPPwL5ljAHW3qsfHE2JV6Q67d%2FxaQrx7KDbJquiQi%2F69DNp4c2LU4Ddg6rcYpGjFngOy%2B9hf3KrCho%2BrtXfmFjbUWmfeMK2fZdr%2FKRZI7MNOq58YGOqUBJV%2FsbFP42cJf9sSd6ZgwZD4xO3Z6bavg%2BccrzGU6qdtKrCZDzqLmurpDbRvbGP33pOkfGOCs9hVbDSxXBGdiBcDkU1rGcwoBvREuA1zNYPKaXKBWN95IOZ2qQzv4fsbVxvfaBp0XGbT64Pk%2BbjsqI%2B%2Fe0o%2FVgdOveR3YrrdZYy4dRNZa3vkAaPaBgfUWaicoyV4SExGDWXK8PJfv3NjCAEozrz1r&X-Amz-Signature=aebdf4e82e9e7301f6ea5b6d60a9d6e1071dd64a89e00fa92eed1df374a0582e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

