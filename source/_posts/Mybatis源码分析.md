---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662Z7NKKGY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T200038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGsaCXVzLXdlc3QtMiJHMEUCIGBvRQTFT2e2nv2Aiefz8EBwLFNzsG8n9mTobK4msEq4AiEA7B4p8PrnETzR%2F%2Bq3K1dQKGpHTfz7ZtybCskWBcfd4DUqiAQI9P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEKYJP8BSu%2F0e9uzXCrcA9iMdH%2FCFk7YL8pmVpD%2FudfIo%2Fe3gbT9bOI4yInPi1oJoiSAHg0RDCc95cuORd58qRs7NFtCnofqIfKofvRKvwcNfqmfcbdu6%2FTzi1DGDr5A76aVfD72AUiW4WO3K%2ByNXoroy6argxMVQt5CKj43R7%2BB4ww%2B8ThYEUTFMz%2BQlxKYtFmdsb1a%2FRrDUvhMZ%2FB8D5QbLEfTHBEkir53fX7I1LaiteduTR4ee5EHWp4Av1GodD7pC7BvKsxcH2ajJ414R%2F01BDpYmyOw%2Bh9uKfu%2FJZHFn%2BIQ7HvfbDDKuJLe5RHt7uv8g2zJZiA%2Bhw%2BBdygLLYTr5KZpzy4R0anZPglRmr0eKWutBwf%2BT7sc8leJHja4iR6nB%2B4BqO7akA5JEuauZ%2Bl72sZtaZ7wjAQSOjk5HzGSd0wjMeT0zx7ydubV9yzwXv7ylyl4AN%2B1pGS2nqKn7Q1DsdmUnzH7Mfj39M0FrYgrx8u9wLSVKfXpVrNkL%2FgsKB0RnH%2BheLhbyRF4XsE768zFixp6hTuIrbUynHFa1WaJCemr8NVGqiXDTPKQ382svAf1%2BOxl1drJ3cz5eieqPRz2W02FnIC0FCh64yKNoMZgIr%2FrjAHvFXBc5AU7B%2FIfpK3RGQUJ5MBRj0z1MMbY8MYGOqUBZjSeTDJImOJ6gHeXHChAntw2DCPwUL2%2BYq02GOkb7uYxp2uEcVJs042gRbeaZzMeZZlwnZk3XiVfvCDaMv11GVzTpB8mob9aLxwVCi7omeLMf8y7WGInYaGtN5KyJ9u1mm3vczROJBPXfgFfH4F5U6loq1dUohZte1BCs%2BxWPH80c3d2W%2BOIEAQRgCKtSvgIDDRVbmrPkKNV4xVxCPByo6J1t%2BRR&X-Amz-Signature=108e79fcd2edb982671373696bcb43d15d2637848b85b09a55c7684f4f6ce2bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

