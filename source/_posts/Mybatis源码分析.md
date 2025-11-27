---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UVW22QKQ%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCcLbe0BocKFWiwi3ZM7jOtLf2yLp67Y6AQhBKNnV%2BFEAIhAKZSe50%2BEqW5Z5Kce9hGESJtWLeM2gmYT9lQ6p425yySKogECJr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igy0W05hD71lzRG2mWUq3APYcO%2BK1IBNfCAwD9p%2BWy%2FH6mkxbwSn28ZcqJ0MC%2BwUBl76d574wkDBQ9LCkcR4ZLfdLOixFHFxkBmYqsaT5T2aQ0cWw7zNxnIgLvnvukpHefOLSDfREcB0k6Fd7DKV9BzxsxD%2FPdTwS6f94c66Jb3dwZ8bsTIKAjvMS7n2n930X%2F3PDYnePWuSDRUaz1h04kNoZEhAP5Zk8dn2a%2BT%2B0hWJ2WSIlkENBFfzkWxdvOhTAtXQChvzwSRxodszud%2F3YhHx3b5HupWKHAywTNGp2mOHb5DCxtZ5VJ1aoFAvC2NylYJy3VUsYoqVnQFkdDamJHgw7evk%2BG4Wc1lD0cxB6BqFvNOdAEZczXsPV9j0nDGNKe4G4k7gyDnxRt87IHYWCZ2QRkDf2MEyAhuY3Xc56atGSw3jEp9oE7kGHllhCGOubooEmcDqFAdD76fiyYV35jWba3fiPwCxjzslE987dnc9AB9sGaSDpUlGq73LBWxj5Ars1L%2BmErSci6EpHRQC5hwguSczdtg%2FH%2BSa89HuEhbbM2JAXMh3P0NNFjy4JEuTGbA%2FWgUAYmKDs76CjW%2BcQmJdFxvenBtZqi%2B3ToJ1xjaIGzUoSdglTWwOltRVi3F%2BIjgOdC0RiRhpESkwMzDgoaDJBjqkAQuMcQXQLtpylAP9uW2%2BDcRFJ9Sny0ftaVsFLD6zudarYWamMOtMPiKvH2pZfYKAhI8awIcwX%2F8gDw3qrYcmFBjDO92GzTcL1aWkJLCbekUAKJnAXY3oW58LPRxtWscB%2BgzvuvsYjlzrxB5%2BHiu%2B7ogQVNbVRbSEIpFlmn7TJMfcjqVchlbXtwJbZltgtf8wd088iE%2Fe%2B5ijvMlT%2BVcA%2FsY0I7Dl&X-Amz-Signature=556fdb5d85a2d0af4e1de31e047944334d0092d3ee89b89ced93bf8f78e377b7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

