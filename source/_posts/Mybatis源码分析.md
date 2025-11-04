---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666HABCIOW%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T140047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC4pWlUhR29xWhshMGLBJaFQ04r0rGIqWY%2Fy%2FS7865cpwIhAIXZTsBqLJYTByg5bOMN%2F6nlX%2FuBd0bOxxcbux2U92nsKv8DCHYQABoMNjM3NDIzMTgzODA1IgyrD0tCL7AKjAmOirUq3APK07DYWljlGDJRAbAHSmSZhG5LkvUrL8HPnHNITrKgOOYlHCroxy75f8CoLAOY4v9XTsFUOUtu9vkAI%2FbhdsxoK9u8jXMwObCRPBifGby%2FSirCdHOPBnI80e7az9cWxdbriJZVHOiGkMScj36pMeApzNApxScuUgr44YumoyuPVZR8agHHpgo5f%2FaVUAJJNw5usn1yHdw7qfA6g9Bm2%2BgFVlA6WCskUHzk%2FyfztnCyrOK8S4XRmziM5yIVHgJpfgW4xvfLAD2vluMjRW35tUfa%2FmfAt23996mS%2FR%2FSXt6CFG%2FUXYkBJKz5Ys6ZR663zIDcNKBiPuvg2tGXKG1bViixr%2B7qIjZ9Dfs7e0wj8Zjoiq22nWitFrKxHcSj4o%2FSiMwLT2n%2FwqkL28x5b6dRzdGal55b03VEtRpdNhy%2Fk1lw4vVpC1yEYF8VQS%2Fq8NVLYPXJZtuPdXQdF1PSTmLPEJEMxghs0dejk3kayEDRVcH3FNVc4HCInBSIC%2BcIJALAeGq8i70axLtZoPGNyhahNGGtifDlVGc3Sk%2BRw24MrFZvusogReO4ye%2Fv%2FkcDoEXQcqX%2BTwUwJt5zuZoZxmLflzzmx6BmP77T%2FgDdD1eeGgirmWV0B7tv%2BJZIPsvkvTCg86fIBjqkASILzhhmHf2gWPjbzKdS%2BDa%2BY%2FCH%2B7ZqzkCjTKiJBu3LEa%2FpwhNxUbBWVy%2B%2BD6KOjr29WA1SgfSq9fxeYX9c2aSFSfNfeDZAOaQ44aLjqpkmMLJuoRhztyi6OGGq1UGUig2xifyzMwYZyJF64N%2Fx%2F%2BUvwAZPYl1C7lJT0I%2F7%2BPi72NqUlylHbAUPfSEgcInQqEOk1oIuX7obPM2woJxOprWbPOWw&X-Amz-Signature=2d202fed1eb48dc0c3832346651d26566353f2e46f25b14067444681d21f1074&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

