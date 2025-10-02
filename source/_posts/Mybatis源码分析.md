---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667Y4QWTS6%2F20251002%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251002T070052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEI7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIDqRQHs%2BJigNVpSOzt8iA%2Bn8NKQoQpequ7N%2BRAbSy6gLAiBP158LfI04127Mt4O72d3hB0TabfwK%2BpjY%2FJFoKIP6tSr%2FAwgnEAAaDDYzNzQyMzE4MzgwNSIMufgMDwCB0%2FVJXwkSKtwDx8NAEWwvU05GkNFUAT%2F1oGKmmNGeArSmgaZ10YUaoDiukQPiVWECR6ppBQhoVza2MioQxpnQO%2BYMpjbCmHfPAt1E86XsWJ5xtvJjfe6qkAMBozkgMsTWYFcpQrFj60Hg82GFcQM05qATu%2FkFDx8%2FmeuTxh7fthQfV8arZTpX2xVup86QckcXQRULV0H%2F9Z5r%2BISOJRgyIQPS6N%2FO3cYF42G0U79hIbVkTvUWJ3aXxOmGNKCy4wAAbYE3t5BP6YO8XSGJJn4ofhBEMEmkx65mRh6BEJuKt%2FIoh%2FCzyv4zzjBac1fEDARZqgrb%2F2HobKFwfUEftZxvZs69mY0e3UwuCKkW5DNjq6iTlAowVOcvWCMlgAn29UoyAJBsTHp8ai8Xr92xehLWuIa%2BiLDKA67dLbmgOCgWHvfWs0ViqfyIDiCQ32G04OG42qgWrU2NU%2B6buSgj%2FE14GatnyQwwktaHI9WxB1DDhWn3eSvCuxrVVB6c6tNERHhUUNRJfMSb7txnEXL4w%2BIboenDxdfUUyHO3xCyAeCFJAlAJ%2BWJyvlThro7c1GUC5xbqdycpNCu4AUQoF92gn8xsDGF0MoLSR2Gc5wxqgdkByZQa8tJRLaX16RP0eDjeOZyA0mnl%2F8woKP4xgY6pgGgqF7%2FqfwKwk696lfzJ0to2nC55A%2FdGY4HKe0EXUNft7cLoAaT2Y9l5T3iQbb9WBNe0lMu1EzkT9RAv56lzJ7u4maCnTW2TJXo0Cgu8WtTFJTOLLJ4bjwdTrmwDpd7xKlyGDoSa7DSpqwnrxgQBdeE4IJ9k94BV3fkN3H8NcJQPqks9Xx5vgXbATNnGl1OM0ZueS%2Fw51F8aYbNmzXARpyhqCfLV2K2&X-Amz-Signature=87520252b29bd804de9d1564ffeeb4b1a7225d87972028d2a22926b69608b25e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

