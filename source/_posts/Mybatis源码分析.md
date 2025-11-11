---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Q24P64HX%2F20251111%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251111T200044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFwaCXVzLXdlc3QtMiJIMEYCIQDH%2Bc%2Bo5gKh2Hq0pu5%2FGVXnaDnvmBB%2BP1wZC10uS4VzeAIhANipd%2BauJ%2BBu9Sfnbr51wBMNXm13uYQT53%2BrkyRcmCOTKv8DCCUQABoMNjM3NDIzMTgzODA1IgxhKrCnGxFzhRYMIsEq3AMtajddmhz03TCimcn4y040GhZ88QCA98wkErWQ%2FwBjh4OF4S%2FalgJ5BruJurOeh1tri3lJ5OQFD7lMydfY2mas7XOs4jtNjzzwdUlrPsrihtvTayYYeTQ80urTssExHJxpwzDBDtwZmUq0yHWBWaHJMffyWVJjm4ULnJWN6t%2BUksNmQF9sifnILRm0l%2B18WnfmAA5iv%2B9Auo7rjPUWH1x9RcLsBstIQ8Z4UqVgu1DQIWHEuX2E4F06h6ZlzE5OnqeY8Dl63kDvtLNQYX8PZLtL1ayeGvWdA3Go8vzumfox58xWgbABwcq1IvCEuuk8MPPeNcnpsUb76euX%2F18oGQnIgt51R%2FbrL3n0qtP%2F5rsAd1RbY%2FWj4C2Y%2BEvBjCjgnhFQHPCs0G61l%2BbBij829ixp1aTtBUCNnNvrJIIWe2uR%2BH%2Bgifo2LXaZLEnNvlxkRhKkPFAOSQdDmnZFUC6Bh0y9prO7BQjBAYY5ongCiXM505CrCaXAkkMZnwJRmwoBe3cMzDhJ5POZCs%2FcWGyReIQzLBnbV4QnHjk7B040UcEDmyuKfm%2FjxcLVZCua6TCmk0zdFRN6JLhwfAoxs4nZgzlPoByCUFgGwslG8SyNBePHCpSbPsVvXaj1a0s%2F8zD0p87IBjqkATnS%2FKh6npjU69aUO0OiuU1wcTmKZbz4J0kdr2z1EtfwNc50Wpky6yJaltE4J91vPIDUUKMthDhEWnFno74U5V2iE4omVB2bXt1CeCFD7KBbgp9cTXitg8%2BUIHNqIwQazD9mgf8%2FMFAoamCTdVADMsc%2B%2FQNV8stWdVooqQPRW99LuDn9TN8OpeQRg0qpct6GiHL7nIegmnp0sunYyCRBTcDICVc9&X-Amz-Signature=c7e8c94855674a3c2916c90b4a2e0316ebc4cdcb7d84d2d9cffd665b7f281acc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

