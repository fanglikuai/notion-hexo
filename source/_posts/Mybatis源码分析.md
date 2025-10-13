---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PS6HS4N%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T130050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDi6%2F0r7VjX5%2FXGFCO7JrSaEBtV05Nrg7rxfj0bxRMUTAIgUkRiZztXuOJsbF8olXiZxZk2odjzg7aLx4tpR9j%2BoeEq%2FwMIRRAAGgw2Mzc0MjMxODM4MDUiDBxQ2TfBfx6Ay4Vx6ircAyKZqUiOvr42Vm7EfJLSpOh1cxe8oHKkInefoOcX3B6mNGM1GuvMuAySkhTucpaeQogJfwlSLvesfJ7SM%2BG9cwae5huS6pWyZb29%2BSkHr%2FYAaDlhJW0KYFjgB%2Br02RnCc9Dl%2Fma2KAgQ5b9Et7cN2vy9cEqRjt6XStlHE7pmMgOhjkVIlHt44isP7HNoCLUkRnmGWzuHmlQ4c%2B17JyrrK1Gga6vmyYixxR0SdV4Xa2RY74CFOMioU7Xe4okQ6ll%2F%2B%2Ffg%2FS87NGiOE%2BAmNOiagzwybinxt9OS10FwDmXvGL7QbUyjQC66ufeCDXrWVu%2FYzH83h8eKjjmTCUxbg%2BTF3s8CnHYAl3WXFKfq8LI1duaPczjvygoGReXXtnNk86vuqqRE03mrY4bdlvyOF%2B6j5AHHvP3A1IQrEG4sZXbw2K7guGoppENSP4%2B5j0Izl0F%2FbkJYsS62ki0DWWJOgzyafzuJWrhqL4crO6ONfxv73QwB2z%2FgFKr6F2lTgRS91ff75NLH48pLCYSAgOiRAs5br6kAe1BTV4XCbKt4d2G7ca%2FPtmB%2FFIL%2BljZvv5zXx4TLolKCJ%2FeykMF7mCJ0CK8GrGDxtXrZEFe%2B8PgxSI3WjKvLsdyokavHLNT2kwjSMPzOs8cGOqUBCzYqQ%2BrdSljzamLvDuAN7YB1qpFETgKZWx09NQDenSihB7Cup4P8y5pWImozoWV22D5LNmzL1lkNt5f8QbNqdHN0Pt79863ZI61OLCQnb8xvljLnalvbb8D7NrEMIn1P5aoOEPstR6iAbflt5lcgyOzB%2BY7ES4401jSUAJ40gW2B0X2P%2FAZuYiOzb9bTj3K2O87QVrBe80KTTzZrOZq2DtRdW7KZ&X-Amz-Signature=af7acaf94258282db472c3d84801ad78a525aa2e83bd28f70825da6900a2b179&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

