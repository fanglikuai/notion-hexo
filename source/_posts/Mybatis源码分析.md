---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665EQXWZS4%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T180041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJGMEQCIHSauRAzyMgEUfURhBVcjzJ8BU4XQWbChBGrrLcfCoH%2FAiAOaJlpbAIae4nSaUErU19veHANapn0AWye4C3w2aYYhSqIBAja%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMInPRLUbS07v2hffdKtwD8G5iRRzX77WX%2FcIE4Bd2oGViOBN7OR%2ByljwvxBlZx8E9823oaHNYY%2BjH8sm1pMuVIp6tRld4yCi8xQ3fFjDLNyxZT7Jp2nNg6NZH2jfUDxnljyDGLVMOSHsZVKTKuuhvLmSGwy7auZzo6KgogtlPhRpSrIhFq9vlfiIjp27RqrlGCBz2AI3G0t4df3EAHIkqhNr8UyP0t52c4OS0UExj4NLLw7rfWKUGWSz6AP7Ja%2B1uRnyFIm8has%2BjfY8leff4%2FspnpnT4vx0vdqs2jVs52ObADNX%2BU1dpV1NeLhZlcCtx7LEzz9FsOwP27mAdPZ7VCbWjZ0pE%2ByV1w8suHgWgOiYD%2BRk1DbsEuLuWqy%2FBZle%2FZpPO%2FFI%2FElDxTqjUbwwpi00kZJLAiIWA0B9KpIuzhuTnjlZ6oDEz6NFgY5rAciD9cxbSaGbrsoYD0j5NYYutGG8WnfKeKfbEYz28%2FhXDvf1FlYytE5iPAPYqfuDVPhfihPDNz8InUbWd%2FY721T6UqJV02lnnMsmy%2FWd5XX1A8zEtMnN%2FzkA64zi9otpqvkuZroz62TIRp1xB%2Bp0rofABPYuMRrJACn2TmEzGrbfMbA%2BqHeW%2B7NImWw6TnkrSsTxseUHVXDvEdhFP6HIw0rjUxwY6pgHzgVfQ3rp2tK0JWqQBZuf7vGk2HK9T%2B4GzFvcfA8ND2X3UDwa4b%2Fx96hyLNTtQPRW0sOarEvElOtisr1m%2B%2BRmiJN0PoZiQxpH8xVDX529YrEcAXpvDivHS98G4qezDGt%2FbIBP5krKpSM%2F%2F82bDgsJAXgvUKL3SlJcX%2B4PufgFGTXdvXADmhG33D6iVNQiBbL96%2BUKwPgFZLnBvvKKDbsZ7Cqih5vyW&X-Amz-Signature=97aa6f60db8d393013598234e1e97500ba3ee8410a655efd4e0b6b2bc532d838&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

