---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663EIR6OLS%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T150039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCSkIalFRpLYplhnrbI3Jesvoj42Bry1pyu56gIGChyWQIgNJVT9bl%2BrsJfNgNz2PeVadp0onlHvvGA5evWiqQ3w8kq%2FwMIeBAAGgw2Mzc0MjMxODM4MDUiDBts4vz24VCqCuvW7yrcA8rR4VfalpSy7lcsa%2FkTKJd4ilFzmkgXdfjoYeboPrQTDR%2FvqRAAT314dwQcBOqNpgv86%2F1xJK00Xk8LlTv7E2%2F7%2B9AFsdi%2BuK44ZIZrDWdFS2bYYveid5CLO5IJ1liGTZH6nq0lm%2Ba%2FB7JIqubyngUeTTr2ZjPMv1zs7btYeQVvkRCVr556ps1VTWZ6bUSSaVTXe9NsBpfWHO2DpUZhhlRd%2Bj%2BWly2jGl%2BOfdywtXR2FMwH3jipobZGPrZBFCcUMpOOLh9klpYXHFW2u%2BvTuL99wlpbSEB4fazC1HS3pdoxqdNfXwDHEOc1Z5Alf5pKNOFk26kgVLkLAFn8P2nfkkNpR%2B6biH7UOpO5W5nEh%2BH0QXnwRI9zRmzt%2FvzTOAUf4ffRZfUZgsJ1zWyAxTrjvOHCtJyKE53VF3aT4aBDJ1a3FNXdGR0t3hh14lVsSbEjqvn6TX57XoONTxeb5aPUNa4aLgMT%2Be%2BZQbJaxcwQrVOlQeMQ9CIbJVzbMm3i%2Bj9Iz02f1vBLnWhp0q5v4KhO2%2FZO3qCwniRpmQuIdUSDtMjCs%2FonZuC5NFsqe3kIDlk1fvBlasuEIUoM4XYrgYYKQZqzV20VB1zNl1cpOC5JbIM6Gh43el5IUn5feai%2BMM%2FfvscGOqUBdDZW5Z%2Ff63J3YdjBlJ9vD97BYJZ1Dg9mtCO%2FhrcjsSuzvatKPSy45gp86yTAuCmjrcth0RCnkx5PCSd%2BqVawP%2FXlWkU%2BBnVGbC7KVvGLrE72%2BaLOt9%2FsbJbEuKIYWs0HFwwQZLgjCv4e1gDPmcmJ2DHUxtGqu70Z9odaxi8hC0IF93sKAvbI6iClj5%2FgvrIeFCrTx3KbCQzp1bviROFnNN5dLKOO&X-Amz-Signature=f7b09c289601bd071c76ba024d8c597ff482330aaeacdd0bbdeb2c996ce118cb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

