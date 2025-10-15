---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZM3N5KH3%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T140106Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEM3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGzfGGdDa5hCLC4RdRi5MXwDlnV32QNEDPVddrYDe4RAIgTg3%2FiQ1D%2BHvZa6kw4MTvvwq%2FTXqosoy8fW7De1kRwlUq%2FwMIdhAAGgw2Mzc0MjMxODM4MDUiDP036WER%2FXVV9qEEiyrcAw%2BYkzaaMkPTXfuF%2FftogJlu4uetnk%2FVYpWmILr5r79qJg0J5dUlL4yIJdSeb%2FotZN6RcIof%2BFvJp%2B6elUKkaZ3FA%2BTDG%2BB6DnFWpV8bvuAmQQreYQTjxie1ZUvq7%2Bgk8%2BqTOgHI%2Frqa0nD%2B6omBmAGGIKobiow4jlih7sl9ZjjozTEjO1FWO5uVAcwkVr0p4MvTfa2IpJEZ6gq0SeHDNwGbZW2G8IsWIV1l%2FoqYeJTz79P5QKguRIHHAkeyKXKG4t3E8%2FNaJF5jvSFfMOA95cCd2CrRObuFXG76bE0vHKcWuNtMJFYPyrXZ1n3o27bznNPc5tnA0u71dmA6FfDSDuxZ8OhQ%2FI%2FzYDv%2B%2FWFMN1At5JObwlNoa0GuFFLEmaXMgAqFk06FQSORbVqj%2BbD5LDW2J3EU1SEz5E6inKDPMKnfZH0IFTqpzPKH2lBAtIwRLIB8e5WKOxFyFDK7i22nmUCMev75NEegs6wWyW%2FaEeslDpAjqWadvapQPmULOgRKyR10o%2B6BKqYHz79AoHpT%2B7A8d5b4fPkqVk%2FpehQUB%2FPVpYu6JGKOhJ4RyDz3TR6woT2E5TNz5douxcZ6%2BxIsmlfsShfvRn3%2F8kb2Nqv8uGaMr%2BlXUrLbtbo%2F5gQeMLe7vscGOqUBrHvo1%2Bh64%2FxRVjtOa%2BrtYdiRIdB2SQ8c2sP6DSmyuCprr7sviISSGMRBq7dQ48AXGWBbrg6Io1pPzvEbgqiguuIZzd8enYHoO9oC4zxFW245R%2Bvxf8qNHBBeOB81xlpIuFgfp7XM%2FlEWRVFxPIgxBtnfj0PKKPwMipBU%2F4x2tvjdw05gDPPD5dqWAeu8%2Fi4S4wq2U%2B2wlYNug5rFzvItHN2JjDOQ&X-Amz-Signature=0cf7a63096dcd6652aa2df2ee1a5429df6df8253a6d18ce2bbdd1f60a4b2ef14&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

