---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7NTRTZK%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQCOvn3vX5gz28DJiOcx9JN9yix1fsogpfImJdIV6sY%2BvAIhAPCzUCFbFLG7D2lrvLXNkD6Xdbz2oPC8ieAJ23%2B5%2FYRPKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyfLdgTtpe4mg4Q12Uq3AOINgZp9AxkXJRnPE%2FvvDEVKxuzeiZ%2Ba28WakPQ4Bp7s1t5EtrwuDNSmVEKNBD4UOtUmpLS3KajmvQIH2zfa7GwaqQ84pTRsvRJ3K1jDB1%2Be49F9NX9t9NzzXMCUhL8lY06zAnhpekY%2B9YKXnEvibYwoS%2FWePE6oVhwL58qJp5AR0q4iyHkrHyuKZaOSRrAztk1FkfZnbtAU21F9Gw7ZKvLhL3MrJW%2FdVbe8l4LwljSF3ivE%2BdUFAneKjn%2B9PaIg%2F6ZRV9gw5tvWfLsUiTk%2BTVarhZ%2BISKb5OwNgUSjYGjwuWh5Fyh4nN9fniaUEILAFqlgtLGntPwsP47csoyr0u8hQjBNQqn6%2FN9CZQuYv%2FGg7u3VzfTExA6iR00jZLfJbDA%2FeAKLzvbJI1l2z4p7KOBcFcIvU2YTxIgDM9E4XPWLFjQE7DkwSadfgJ8vg3a66x%2F%2FvUw7CFmBXDTc9e0MAcf0nyWbK%2BNL%2FaNsIR39g5lnTk9KIJMda6FHRKeyYOg7JL3%2BquaXjNN4F2Tt%2B8D1SNfDFOSo043ODITEwVTVZKc0nEN47VpcKOqRc44yKu%2FroOIhj6D3wylODW3%2Bcq9eS3nCA0PyNGmoY2ko1gmWj8WMPUYZx3pgBVFTSbCq%2BzD85YLIBjqkAbXhKg%2BwvuMdkqw4BOFnNrguT5NhkTY4%2BSQFFDnWv3wVtVkNP%2BXoHYBA%2Fcocx0zkChupnCwqvEan7lDafPtmLznZ1aBulV8uSMCMkxyLzVBrnkM6bmL0kW1Ag%2Fd9o3H8nIlV3nItR7n9%2BNjzwukfUEHIQQVQkYBIaH%2FrUgrVNpSJ1OfAvBhP4Xn6jsAYbks4lDHbwnd%2FS2mqks046na892eqsDp%2F&X-Amz-Signature=32df62caa74c1f3f8d59e3ee1b6d584a1eba2f618f96a9593ee934b756d42051&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

