---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663ENWCXTK%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T050043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE0aCXVzLXdlc3QtMiJGMEQCIDQQ3MrjUpvF0I0YW5zI6kwT7jGUe6AG7t1v%2FVMDUyNjAiBBkvGBBiUXmNbrLQCvZwmmU6HB%2Fkeo8%2BxJzLnDDdl5fir%2FAwgWEAAaDDYzNzQyMzE4MzgwNSIMkXOaTkB7yrJw0ZPJKtwDKHD1Z3eCuzSkPSCSY%2F7Oa%2FnLQuSV03vv%2Fzyf%2BE947FEo72eL%2F%2BKTsL5Uhe1eSsDU1W8SPMoXm6NjzqhIkeOwV2OcHuQWhuaVLf8bEIqd%2FRaeTjmA%2FksZu%2B68tOCYMel5x4FshxtWR%2F%2FzXf%2FMeUXktKOGu6YF3SlU7rS%2BAoMzo8kV2glmKLRG2yn8fCOHtQtdGD1mf839xpJXR%2FDC5dz4UNw%2Bwk1tXKNaVHsjXcTr98aydT1XjQkV29B710MUseHdQSC3ps25MvphGr%2FXEye4gdTEqg3Gz76%2Bv5w493CY3LXVbN%2F%2BBF3O4TC%2FVi%2FxW3kYQAev2ONrWpxS8iUdA9mrwHICCzj3xsHMOIzYueoI0KuyM0sfXRiK%2FXmTzzUqydMfJcotHNIZzwO5T7H0Yz8D4tW1OLrABVw73N%2Bx39LPQo6ntS9zKsJS%2Bdxoj7Qod4dAvT19jydTWyTnhyZtSgEHm%2BaRdGSB4KRis9O0h9h48jrPOh%2F2mqcvUaA7T4PWYQTRQuk7AH%2BNFBX%2BqnOaToUvCzJ36kL9vqZ82NBwDy7bLIHPgA49YdGHf24PNjXBBDTFSAR7Qn1f7vRQpdr34JQcqa8foNd%2BXhF4FQ0OeJU7w9cis4eK6zmZd2eydv4wr4TLyAY6pgHViQhpA%2Beoq0FYgCoyI04SkHQVRmHQ%2BE4qrM5dsFgLLx3gG1MKS6w4GVyhSeVT5z6HApGC%2F441YZWv2%2BkrY%2B%2Bdq4VA392iHgYeCYk67qBf2DHP1z62BAk5kbAfb9%2BXlK839SGT1Tb0M%2FJeAj%2BKBNVIP7bNgV9NwZV8gj%2BdHEZ%2BFADjLlIdK%2Fet9%2BXxQ%2FU4H2nyDSUhDGq7EyJXX9NgeOcqmGpHoRX5&X-Amz-Signature=301b9b5fb6416a8e2d41c20dc432049a5151841cd9ce90aa969962631f6f67d6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

