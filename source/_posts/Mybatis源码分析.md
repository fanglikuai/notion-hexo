---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RMTJAVMB%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T050222Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDjTEbos6RK6c5UqPFeUM3nthS6es0mrVpTAQ3hTHYhmwIhAJ8%2B04bBanuuiCc1q%2BjZ1956oaDD0NoCzt4J0az9UgOPKv8DCCUQABoMNjM3NDIzMTgzODA1IgxznXBwV%2BSuRe37dR8q3AMRqA9O4vnK54b1uZc5kINoO9SodAjKCCOYGbtQPwwAOkhW8p2GVjLqeEr7QBCERVHy8EqvYe8iVDstscCvBm5pw2GRLSJumXo8K0JEc0LEtCU%2FZDdcMWGwtQUbqSiJg5IGyegF3YfN%2B9aHeEX%2Blx%2BDp%2FRkiWhloIuIoRSi9mTnOSWG3aDP5AgoUBCHPwZcgV899c4t0m%2FU1PoP1Fp99Kbhmdt0ozp28gzieIS3nA%2FfHNoe5ni98vneStK2Ku93hOvQXsRHtVMjh2VmIiyfKUQWtiqllC8npkvvyiMMZ6qLO8ynNAknLD0pYWkAJ3Wsd43WicqrAfzNVJkxjwQ%2B%2BoYjvuIacm3CDMa5HyacNh4AZgdQuuwMNuyDtE7Yqo7X30VILx5q73eG9b%2F1i1%2Fp3j3R7XlPRAFo8nbMmk73G3%2FxQuC69wQBycIe%2Fgf9teDx4NROfQoTT%2FsEMjDbIBVYxbb7nCZRSYaMXfhxgPWzeMgLVYZeAdXynkTcQQUWJaoZCeCVNEHtdwLC7lKZFEpm%2BjEmONXxpK5bTRUf%2BFogvb6xZmqvbCplVmhpARaAOe1LxmHeLogAc8CNd5aKb%2B%2BlKp%2BiX1j3i3AMaRSVVEVM8DAAp4bEYiGm7QM59%2BN8zjCrk8PGBjqkAd1ICgrQut%2FCPMoBkH8ufNdt5wdDPOfwp6aTUEireLC3I6bFzfpFjOHCT4fPycsggInQgc3DpAMOigeCKSMtaWaxBtPTr%2FP6QXNzfxJuY2a58bHRdpv5Ox%2Biesvn773JmiW%2BNyafCoanuJAOCHxNsKKHjLKT6LXVH72v5IkNAoLH5%2BD6JQ5M8jZNAEU9qAT50kc53hM1642ggJM01G70D46rjePR&X-Amz-Signature=1f4ffed16a84ec0b9f09d364f620ce2da67ca052028dfa2d6d4cfc50233aee51&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

