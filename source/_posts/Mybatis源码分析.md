---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S7ANN7B7%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T000042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDMT0mkuCdKtxFOx1Spcnn3uRfmW6Kkp8cHGh%2Fwwkbb%2FgIgbtQeBw1RgyTlCtPMj4ZgHfCnvrOhmA1QDTTl%2B5iFYEgqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGiFym9BKN84MX5zMSrcAwa%2B%2Bfwgh13I9bKNTGd5t5YYMW%2BbWN4Tp8ES4mEnx35DrWASPCK0bqREoAuPabHvEOfJwAKjhWnDBG%2BtreRrzjjK%2FGZrHkmo%2Bjs0wuwXDeQAqxgaiOSBnpLlVfHkxr9YSw2xniB2ywLQHRYmW8FF3X0Ujlv8dCe0zg7%2By2Lrw6Jwrief69SptaNvON0jeXPxBU9PaEKWqTEXNjSqP7WUvmZl2g0vKHCY%2Fw4XUI4KlyepdG2Azx%2FFVBQokt73KKVU%2FmNDIM4iOb9gZ8uTinPyqa2BhHMZPNdBEIKDy7kJlvMDQ%2BBqRoKVaDcA3riTRooqxNTeTBvoFhzJQZhSzU7rnRJRFH7mUNmpTFNaJp%2Fi%2BFQFm%2BTwPaNn7acD8QXEHnMAnuJZ6jHRvxL4un8IvyAu1eyIg%2BMkKstb7EhOEKr7zNMVVLxQWLg1XqqFtG4irbNlOTge5kLCpLxLxMQf3LtzVsnxrUjUBQyZQAp8KBiuNbMq1HvutDinKn%2BMpSfQfLPoIV9PXW5pCQgTfO3rbLLjEaTvFsdhzd8%2BAmSF41ZwsDnyQ5uGrRjt1oFtdVTLjoJIUqmruIgtN%2BZww88XXNmEPuc9osqgWDrLJ%2BAw6bSYUxqf3xApPDvIYLZ0RmxQMJbxj8gGOqUBquuyyqM3plWFpG7UYHC7nRW1xKfVc38u2dFL8%2F2rNc7WrHizvF0C8FjhPwymSJo7JaS8ElX10TokJKtghOX%2BKkR5%2FaRa7noWCfJZk9kz64Iamf4uYGtUkclz7da%2FdmC%2BRjEqLNgtOzod5Mw9WhsieAcPMhh62hrzd5zElcbP7rn9o5IUY%2BT8%2BxoiTRYQxJ1%2BgjVr6YkIF0S%2BxteLhYM%2FpEYwFSHG&X-Amz-Signature=cad4530c98d73bbc600ccac3d81a0d30148d3b91654c2af964ca34882cca5bff&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

