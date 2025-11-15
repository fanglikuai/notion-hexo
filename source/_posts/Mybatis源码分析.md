---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLX2MVEX%2F20251115%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251115T060048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDyJfcGqJKZGfx9pdORbmDKjps3Crsb36FDwIZRqFgnaAIgOsLGPPknhkKIfQXuSTyMpZvr5LgcXMm4DVT2uqCrxl8q%2FwMIdxAAGgw2Mzc0MjMxODM4MDUiDJQQNw6yxa3EARKAISrcA70XMfmOXoOdeH1D54RxPOmeRwCHD6VE7WojGHcGHfetkwhAOzlvgM3c2SWaYTx9w5VT7jrb4llpHmFyNK5nOGCjAECFdhWvXQBYRWJlbJBxJfoZQOSoMTO8bRB62WbseVKoy3CA5ZeM6NfOlQD0lm%2B%2FV%2B9LL7U7uUufJ%2FOgSKl%2FOc5EgRIQ1WP5Yxj9tIyDATs44gC6c0Bt9V%2Fm1YgLTwZ9fKdFQXPatjoj5cr%2FQmEIGIkcBUBhloGRM%2BOBj6QPwNd3KwlrO0DRLrC79M78QH13K6p9mOfQL9bGOkCj9TlHN9qWTZ8eVDLyiiuSKxoHRqdIauRQA6a7DWMtTYMcddS2dxIJCqJaBK76VulRPwsYtcMpfQqOYMA1baDcKCD4zScs78y7Y1i9X50g5QS0pNmr3%2F2UmcabTEUbuCkHaeF1gBxPkbxYCUknwgYC%2Ftjwzu5fCZ94pWU6XSy7HMwhbBf4wDRt2250g5ZQgfrZQaODhvOtcXg83SSrEsk%2Bbh9YcBuJPDWUj%2BC0%2BXSSrR%2F6aEDEZnrtJdsLNDgmRF6HGsKxGsXmmmF08kZ5qWWCGfyz2Tmu8WcfbbEp2TPBO69jqV2Zaum801VfKi0drpSwAFWVPkGsUKt6f9iFZ1WgMJOi4MgGOqUBAuCEj%2BonPcq72o5e4RfwQYxvgVhT%2BrXghJcD06kqk4qwrloSDTgO1qBvEZxVpvnqyDdYNy2%2BX0n4Yw8sIj2a22rS3A1YUVjk62gMsdrMOwTqy2VOrhAJXhz%2B%2FQiWHuhd991nkEmsaqUZdV3ZloQDVMutfmCBlCG57YV7336Yo0ab%2BlJcEQJqgv3p6GYFPxLolJd0CF3ZBZnbGZkGSoVhGo1csiQY&X-Amz-Signature=8094d7fd64b8f6b84e9582b7994d4aaeacf43f2347d4a37af4983b4acad8ac76&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

