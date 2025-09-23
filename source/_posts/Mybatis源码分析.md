---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QTBBYACL%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T200043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHnA%2BFx2QVU27oMUxqH69eSOYvV2qXLavRUaChMwhyvzAiEAz2WFyS9FJdAoUHbX%2F5CoEvOeyNCsWHMRo3j8sMMYwz4q%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDAPPhIMKPKu2olcYCircAwvzFxWiDV4JlHkQuEXjID80mGrBvFqFX4%2Bt1HX2JixHi5%2FhYehfRp%2B2oNvQ0ZSSE8PEcMg5hgfu9k2HsGEzqvpGYCTrud5WtjOMnHjseRxLTjXxsEx3EsP%2FBPw3MIR%2Frm3qgDJ9XitRv3Xdr3LJQ13aRKFSY1NsdKUEjOZztFMo98QpgAG2xZHo4OJ%2F60vmmxD8aMamwKyjsUZXYs6V0bThd1redrONObwbYJBofhXxzTMkqBKYOI%2BWr0uxiEJU1hB9cJvIcicpwayN1rvbSjkzXKMfvjcO80XfHfWXH1abyoJk7nKuHrMUPfjUb8IoovMlbeGmEAPsUjlqawqrCR%2FzCrC%2BjUwSRglciGm2%2BRC%2FhUuhOk94AlE1SH%2BXc5CfCcGSiK6Oo0rDTFk7QDqn9NuHCD%2BK8lCme0Ezg5VMANDUF9rJkAqx%2BTvuPI2uO2cYyTmeQk5WAvYdsTvCOV9D38pfsep3hswvAh4vzXr6B1sYxom22GRDbzlCQedMRve5o4omZ2oIwYov5vDkqrp%2FErrv46DQp4%2FPXbdZaiukViOtYcs%2Fd6B1nNm0uy3u5Ok%2F6KmkT8%2B2nMIjs3YwcGqnqEuXvT1pu1UH35LUAdmHK%2FbDdiqsPJVSbmeaA41PMLviy8YGOqUBWURPzzBKZiKdN4vILEzePSU6ZU9%2FjKe3Sro1po576NOSNqUU5IR4Boy7iiun7wu6J%2FP5upDszx5lUW4s5hTtx4TDokTst1mjUYFcEYKbo2kMsCx1cENd7WxBsVAMq8JWywpuXMa6TW%2BPgW8pZNS3tZpNjUz%2FkTfoaJf3zhMwzr1QfCt%2FnxaXrR05bLUc6PW0F49gKNx2DWvxmCIpq43fyNfsyePs&X-Amz-Signature=fb1103fa403588dd216a0ef70018108d59db0a34bcdc1faf071fdd21af3f0fb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

