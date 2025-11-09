---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UAXZZ7BR%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T210041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJHMEUCIAQk5e901S%2FLMTdK20kJyRCtjivt9qKopHtPl7NT9wU1AiEA3x1T5UUaz1bbRVcG%2BXCNPOmKQZwIErqfq9Oc9Tay7v4qiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGIxcbM9fF1Qbp3rpircA3CiZtquB0oCJDKPY74GMg1BpfvJBIgTRdC%2F0kj3ySEAyojzKIOOkJFzncc4JN%2FpvqLTaVQNJKBjQ0CPtpkkWDogxj6AunzVqHF0zkJyCk3%2ByMrTFreTR64Nd0LiU5Ln0V7H2bDCd9bpwXE9HCUxIYBgfo%2B1%2BxyuQAUuZ5vq%2FLtuPc6pnIrE9zOvdu%2FD2TPbZT%2Fa%2Bpz5VKdXKKfWWiD82L1K2axwfEwkGXP8HlKicL22od%2BBZxJST4R5Bwc1Lhcvo31%2F7ZU0Tvc410XorNiZthLyUBdpDx5%2FF4%2FhR8N3ooXoKbUnMWVcEzu%2BB8Ij0uDGXK99Z4vWd3KZ6iiz3wZJqfQxTxcJ8hSO6K%2FXIetBD%2BI%2BtAZI4XQFD8U5NSQtZC9OYr0d1N%2FeLCxGic%2FWVcIuabxev8qRPaPtbEnljagHrrz2CH%2FdDqmAB2lU6q%2B7E%2F1XQUam7KKS9ypbLGejGR5vdbAc0wFkUPUfnBMjgErk1DaPkSP34flHFAh1lBO2PLVcUxq5TNcn2eXSMhXnrYzz5kN3nRuAa9GjQp5lacYapqeaN6S3h%2FNHWfo3%2Fe4NZwMzAI1kv%2B9p9mhMpACWBl9DtyxOX1mvBulFS76rzuJ0MDyHnQrLEHk3Pq%2Bn1KvTMOaAw8gGOqUB4FDLbg2zyEHBeKaDyIJ%2F9jT6RB5UbvLVLkXP%2B2aObaCXZACg5Kzqk%2FfbjVh0NID9el0pcjAN6qfHElx2jwHOa%2BRyvqBsPpms3XxpIcmJtv9d9bcFCtkGI875zoQAK%2BClIEPxE2BmXkCRPvmt4jYj2yqLKSrBsvs7c4v3iOZTJ7CUYgsMJBlWUrqWvE4%2B3HWWRqUmQhUO27vGYUUoJcKFANLMbPOm&X-Amz-Signature=bca1b3f9097c688fa23a746a2ab0ceb04467358695daa3d027752775ee2e31cd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

