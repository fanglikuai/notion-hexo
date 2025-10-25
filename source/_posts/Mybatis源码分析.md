---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663RDNPU6F%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T120050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC4Gb90RsXmSCkeQPg4pX3wCMlH8aDBbSqDc4kQevRhaQIhANRyfaVNCb5v%2FGTevxpxOSfG2NnOgn5Fx2WIZ1KCPoMKKv8DCHQQABoMNjM3NDIzMTgzODA1Igwz28IByC%2FyHpM9Zrwq3AOB2sRAsTx5A%2F2xyiKzE8mmVx7BVhXPsjjWQHWyB4eJ%2BgkTWsN7MS2mIqoX4sH5rmaMLZoiCfQ%2BBEEFoRU8GCBs1d%2B5CSBwKRtLEA3zRiy75BGvAkW%2F6NxUayOqlPJRCLNa7hWS5ZKuO6xkAuki4a12gbgdho4LQZVRPQnuCCppkWNKRIFIFwoXO%2BlbpVgW4WoxsXZNQMzmXKRUGBf8Tdz4ofQvnyo%2BaGOHKXe3g80OwgcaKLyIU%2FFJnkj5%2FABnkWk3mL39Saj7FlghywkIGATkR1vKHsnkjGeaekYwNOWHUeZS1w9Hm84dFqiGdyXyDRajvRy8GQsIuwk8JrdUnEoAkLF%2BsBIPVh1LFNeHXTtiYh7SI4oflqAZ%2F5WBtZIYdRs%2BumGHefrUpg8zrglYtYEwX0fKDGXzRmtsgUMowvDV2AsBKj%2BjR68kLr%2Bu%2BWZU8O4lTnMq0Y1LpXvngy%2FgmJg9ayD9pUzp%2BwSEsjvJcut4bcdTqThYeP%2FSvaG9CqPbCU61OOru8IuGj7eo57nzKQTzLVreHI2jwn4R6mPbr6I6tPHWzbMSNdPJzUwsuWmBgzPrPUO5OYiQA4YNu0NVytHlF6pXCHqMTJoXq7RGgmEIfo%2FuF6uWjnA%2ByE0UhjDL1%2FLHBjqkAYA2RXZUS5M6ec0AhJKppTHCiMeQReMAWuEG1voAdzDgAG0iEMvXrK5MTVWr7ftmhvxeKVrfs8QPQ%2Fj83DS6g5bgkC9hMVyQXQQJixRcYnymjnvvWQBZgKCCBaFGPW%2BHMpAAQSdxBCAPFkVJ1tRewXYfWN5mpwUIwXWC6Q7URtrHkXbRfbcVDVKrWWEPMJorRPcwgvwWfPbYVh0DpySo9DPse9dG&X-Amz-Signature=b2743e2fccc535612c8b56744314da403d8da6b3286accad3a50a77afcd94e0f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

