---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666YLPWQ2X%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDlkcWJ9cFD3smsTVwZMvIRFs7oJKJ55IbEYYf5ikP5jAiEA77nMYobYPDsan7u1aj8t%2FVDrFNLoctmVgA4HWjwfkZsq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDARZNjKMKSEYmUvEZCrcA4Rl5Z4bI0v2iTJqc2xjD%2FPCVO%2F%2Fctbv%2Bi7V3jk8%2FeHbcQ2KV6uSYsZwsxxU2nUV0KxEMxMq6zw4ftIsC7b%2BMmrYxzmgeoAGIscSSMdbJ55H2JhQyaIVkLzTCYbW%2F%2F3QXa90ooE6l3XqN1QEwSxECtoM7E%2BmGCKEH1is1Aq42Z1YGqNC%2FY3U%2BznrqEPT1b1k%2BuOFqErlSc7Efbj3Tf10f%2BHRIorM0FwD6XiAn2VhscXJfcYF7ZabykQ0ifJ0OpEmLu89gc0UiGm5%2BIH%2BDaCC0GuUuhZHS1zAwh1EgJyboPd2TK5F2P2gLPQmdtecP8ESPudnM%2FpFWyLGd%2BRNlR8cQ6RstST%2BZTk%2BpajPEQ4w7%2F%2B5Mn9FTt04dV6jKV2fBpFvSO28XmJJ9HjlwmqZqMCHKVcUrG09dBbiB7DS%2F%2FF0O%2BtWDMQDjIT9MnwV1DlZ0xO3XcA0gDNFZfF2RkBtgnW0m3eru00LUvChy3x%2FZMpLbi7qWs3yU%2FOzEGkVlIWZOHxnmENYKoG4h5xcDRXPBOULl8mI60uJvSNXNJi20R%2F8QNOcLUvYtACVj1uD5Y%2F9k3EQTRUzrKL3PJ2e8REHsFYVYxwOAyYadyGKN4Un%2FhXbv87NJvbP0lPa9tPvOWDKMNupv8cGOqUBfGwekJOB6CbiOPp0pzSDdEHl1VzRHeL4MWtW5xk4m%2B7PmXdchuXT15C%2FLpbp7viQtzDJmHvdQVY1per%2FuVmYJL%2F5oPpJlxx%2B%2BEV8UE2x59ngwkIrsenjgpevhT2hkTqyDNlWrWStSFy4A1vpiCIUkRMmuHbW5UaNE63sOqViziwpfiOviU8xHNbVPvLGG3dF0WB2w90LhtS3%2BIaWDFxU4AVMKOSy&X-Amz-Signature=788397ec9c279af9577f2b3ee4b141bcabe21b218ff7580588628d3810527bee&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

