---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XM56SZN6%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T020049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCICCRQywGn33J3ZCF5SyvCgQPhD4BJoH3Nvm1tZ%2BVMe7tAiEA7TTyGUhWU%2FyZQzHNtJkqxoZaWKD%2Bm5O9Y8dmQw499wMqiAQImP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCTHg8yRexoPLWHUxSrcAxrXKOpIMa5OMhgd2LcLoyNw%2FUJhUW4GAieI%2BBeg6L8FHf61T85OGJnMAlnivzXbdWVTTPZ9iTaLI9ZjDGgg9SJxfPDBlpKD29P%2BcLvgcicJfEwJbopAgEFFSH4bsn635qGWlPcWJXNXQQ8I7EiatEAZxEUWB4%2FHFoCNbAYrWZJpVm3LLFjPxHWw2RvcUwmktIe3CBZlFTbrMnEXGRgAuDDIVZWgstoq%2BmNKPsHDtyTtaoMVTt0PS0x8cutTWn%2Bnld2kEaiS0fUesIaKI6Sb%2Fn1rfntbA0wXevHQODGJF56Ho8ugtR30OgSSPQ7fcnGHiULw%2FYaRz6qg7ae7cSXf4zwM58B%2FKwXh%2FWt5SDow0LpZ4zBTi9KcvrruNyU%2BaRev%2Fzc%2FTNq20fOqLN7xxOosN4sknDFYqxAbbNPUY%2FHqLCLdiCA2i1a50Kit52kiIxWMthwlAf4ezLNLpwqqy3q0i9zTYF%2FujnwaxpWVfworg3oxwJaJSSxZQd7AnwI%2FW6A4vWQhv2WJhl6g8n0QyDucNSzcJ5gQuomM37n%2FiCga%2Fw1qWdqiBZVsYtu9J8ezygklIujUI1PBuD0chsvdFOgac8gNvs4pcLidbS4EMBE0iMPAqDJ13oL%2FUeqA2Y0fMP%2Fbp8YGOqUBZnwxdBNmckSE3dazoH2r6Kkx%2BGfpWX83PnFTxLuwXdCWkwjXbwblXaH%2B4ALtZCSJ6i0Nlpm%2BpM5BATsAfeJfJICVmlNmQeZJw5XWOBCBwvqQeAjYf2n1RXzEE9YimSXmLaMuzbut5CT2bvpBzOcmG6GGL3F5TmaljtfqkPhkKeifulycE2os7DcL8WBOymIUFppCHz9uhxtBEDn52kSJl1oY2WYP&X-Amz-Signature=f0186b91390710d53df9da5599aaecaddc1542622065b249cfc51d9a564e02ac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

