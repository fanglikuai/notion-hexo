---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVTZ4AE7%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T170128Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC0EMtZ01w0jAfZLUVUZBQQP12JwbcvArN9oHHhVWWSjQIhAMjxg44UskBQTX9nW%2BtXY6Pa1CmInWLZjIZohzBV%2FaRhKv8DCHQQABoMNjM3NDIzMTgzODA1Igx4mBP9whf4%2B0SwmV8q3AOPbgX30BnQVpUOLVS%2BDnr1YPRXKizRJqGcMLv%2BzyQzhNcrLzSR3VYWO1LwvnL9cHxzB5fR5gyffweWervcV4xPaMySXUqlNWbKO%2BUe1dvMBjYV4FwQRefLZ4fc6M2v8XZt7iir8ApDG824yWi8kxznBrGKvJrCR8RL20jUtgxg8pcby%2B7Yf1lAONO3waZDnYa7JU86C8OCRctFDGAwZGlT3Kk5SSjhW7SPnr3%2FK1Sw4LHeVwBwcqL6bVHMHCd4Sv4F8%2FcEc6x4EjqccFOjPFq8sPLvXXtu1dBgouqIQlYskz4JgpZG0lo80A4ALdDHVBNEpA2DiZN8z9IP4GxbxExlPOFeupgz6T20d65HLNKvIxsOcgi5uS%2Bh6s0XXTfed%2FtlBGmxf3haXDI6i1HbliH0XJ5%2BtZTFv2Rqq%2B7TSQwnvmXfqGNDKOLp9BCj9Pyrhea6aUbgMVy%2FVfo6srsoD8Y5gO5gDcBmSQ%2BEtCfl%2BVe2iSQcCmLomuLXdeG1sLMM7NiWeChrhaxtJa%2Fh2Q%2BXfRtAq87l8IEgvHyYsCOlhm%2BuGWPLADSFhDNto5XVnPS2%2Bn8oou%2FGpJFQ%2F4AejqSgp8Hxik6xe%2B361cESd7eQLeVIsAd0NfEoGbMWbQllJjC7o4nHBjqkAWKVGAXP%2FxLEIn1COtK1IxxDwAsUbm2zItaD%2FiKBnf1PSg6dbaPV10k1FA7P9r9zHPsSNgHEqAPBz95QK6rAV6yX%2FZv0uXOh1AjShjqN4X5SCY2TzmggOpcy2pZ%2B%2FNYE9pgAj%2BtwLkIAAmoWQoC8jz67od371MPkCG%2B0Vj6RG%2B2JoRPdMI2buBMFzKi2tvo3R21Wf%2BQqbT9nm%2FZ8dKECEE3j6LTg&X-Amz-Signature=2a1689232604f2dea7e853762aa5374e13cabd00954c2579127a6a0675ec5641&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

