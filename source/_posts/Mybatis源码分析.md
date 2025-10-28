---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TG52J7ZL%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T000054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBNeaaatuQkQeHVCj7FGwMHxh808kJAm5okXwvs9Dwh3AiEA3MIPiyGik8PhxdEfYFrTvj%2Bm1SfkNetXGC6Nuat1QUMqiAQIsP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDLg9YWpg3uoVp1cYCrcAxKAE1SZWLIxmU4SdBZIZAFXu%2BnGMKqQDrRBQcLqcGs5Rr0UL0ncSYC2IjWvTjPjJvPgM4dA0lF2lA%2Bk7z8r41TtkSrPe6d4QWjTsp80ShQG6JIiiTWpKOxG5KoRziZA7PWxteXg12otwOgEzNNVe5ndH0phLtvRdLGqmhXnEsCXFlrMfny3kIQcreg9RGL%2Bvu6eAXiJVMRtPObPjEtH2vkrm%2FZW3ZliMK7XmosDNKCyRTk1%2F6pFNijaIKFBlV8ZHwEHjl4Z2EbDTwMEjgoUEpRz6%2FV98IeYHefrmRoZd4eeHVGHrFp%2F4wLu1SEe28JxVftbOBoYfcdzU%2BLvoj8264nllWdMIPMJHfwUwZHECk7Kg6VuXidBFUWSD9nPMGDKpb4HWjUYN9mCaxvT0w2p0b%2BKCidX6ywKU2amoKzmBXBZYmWYcOWutd1sCmFkH%2FdUM03VAkYq6U4ljcEMJLI%2BaeO8y6uoZLam6BRGvlbLtjud4FxrgA30nzsFpmq6I8umzlO3hAZKsh0hGpqB5pHGcC8WAIQhU%2F7o6ZPbHMUPtesmt6o%2FOvwhM9lZHwvY0gDxwH4bX48BIbnQbYcBjbobhmlHdWHiRrck6BMnnayoe%2BMhE8A1i3pkrRdCrsXQMPD7%2F8cGOqUB9bKF937J9f12RKo2Ak08Rs63e0rrkF7VR0FlF45fr9Hl%2BiecIYNpRT45Qhz0ZSQnvt9nBSxzaU92k5kPtLPxzLASEy%2FkNq2eNimqXl2aDcqwdjpEnacLN9EyIgum%2FjiZMqrpVJXQyCyInfjozfZEu76AtY1Pa4Z81whnRqmZlUYPFxxzDI2rblzIO72%2B%2BfGUqkDklFd9FRzFMTPFPjNo3JIPn0zU&X-Amz-Signature=f80c525c4c6ed2e108c1e7739a14b781d3409183d0e84cef632cf826843ddcb6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

