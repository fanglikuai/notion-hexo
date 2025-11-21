---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RNBP7Z6I%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIE93b6eScioPXSIOTV95Vm1uxTO0YiGUjpDFvKLTFTmIAiEAn1VpM7mAAUt7770KndaeKFsDlAIsLlVHAWauoU3MUtAq%2FwMICBAAGgw2Mzc0MjMxODM4MDUiDLNQdfydCv8KeNj32CrcA7FFGo4Ra7kNKCgxiZD6G%2BiNZ6pfqNS2ZyVpyDoMmUNz4jB%2BRCAG%2F7SQ48UqZWpG%2F2QEzPTPCRCnn%2F51DUr1wyc%2Fes%2FptMD0W0M8CMv7KrsBS34D41H7Cvw%2FVWbvWqHHg%2B7SeRKz4bmVm6zop9d1sLWXcajsJyqpe40OlaxE3hkydCw2jraRNpVYNJaPvZ0UBuCyNdJ6y5zxc2KzCh3RgAmpgwmvkQ%2BHwDCap7FneDDa1H6rj7UFc3zBM7HhKfmXpMbe8gDlF3%2F%2BsNx%2FoauHGK43ZdYIECdvFMyF6rR5wfLQO2zE0jd5dX8tiRH0gK6R46J%2FNFd0zoAdGuJbKgYbYOOaJpG%2FVdcK37Sf%2FBNrKcGEWygAkctojBNi%2FGwZ6GeiuN3rAUAnjtzVfdPtYwyEUh7Gu8QyCvRRscFevtL2YqapvsKEmajmM1Or9auCZNSK45d%2FdyT%2Baxbs04wkVLYDxE0F6Emu33ow0aj7mE4N8OIVhnXai52lzcq%2BR%2FT6gvQBJsDc6EBGtlSOxBO0Kt5W14xftBh90%2FvljuzI%2B678yE1YDOEu1mf1bDVM5a9AJx2MeVgpy4Fj4ZKw%2FRaRuwpa8%2BdsN89KYdn5oecAB7JFJWrFXtrZWyvFXoBBIzxYMK6fgMkGOqUBuLCN%2FZkZrqZWDj8r2kjw4%2F%2BZKGhDhR%2FH4QVp1CFZB6dDe60r%2FqSfH%2B%2FcbHIfpt%2BjqVWfw4NI24M%2Ft0QKjw63O0N10m6gyRIXnNZUVRGgR7E7nmulzdjNwebnVfvGju5DEAFhICo%2Fe6MQ32M4XIuOqrcsZ61EhBIvU3QAOn60WoTHEbueOly9uFRs9ErLhoIK7SWWWsIQbviY7pOMu4FZbK2WF0j6&X-Amz-Signature=ae7c9a356c1f84f734dd1530f550343cf0e0745c1192825123f3de447a0a3661&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

