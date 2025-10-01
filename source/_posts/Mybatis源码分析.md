---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YVOHTAFW%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T200041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDb%2B2tzNnSr%2Fsd77aj7LLnszvbx0WeKxDDq1F7WPcR5swIgVmHpPS%2BpiVLRR2twR9lJivONIZAgO1YqvilNPOIA%2F0wq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDEB0AjdlqKlWspAhVyrcA0wXu56wSkfuvDlaJpnL4TpEnXbbGxAH7oCY%2BVfxI76XPvnWrmHxWvo1PEbsYRFhRqHXHXEdee%2B80%2FZ6ZuqYHpSOWp89ggi351D7jGnDMZ3Ji%2FfQOH9cjDOtgpcGfP4XUBrL3PBXX9aWNnACrC6eRE8EVseYDC%2B04iUpJaZ6TKiLbmM1i3s%2B0iHXEDRnQDgUbFt6N9FQMdqwJGnK5Tn0F2XRH%2FC6d9YWDFpuCjwbwQz8vSrV%2B2wt%2BiMmjeUMc6PSS2XP9F5U8Cs1jVCYjnErd75y68RFg99NQjqPhpD9ssTQF3UDIz1Ccs7eUZm4YDTwvVr3DIOaV1HtpdhfbineEw0MymXmHA2yW7t799NoUgdUM1%2FbL03SeF5yCoRGqsMVPjdfZ9UXMgnsXtMVYLTe3t4mZe5y1hCRFJVVRwzLfQADT6ADIZC81PPTNXVS6KK55PdVngm%2BoKivovz97dycZHKQDRHzy0riV3PJAPj2G4qfrmvCmKx4S1bwCJXthUWU2%2BA5eG%2Bk5COszsfBPsPoBDi9rauCBObxstJcUQE4UpRVwWRT%2BhnC25j5kBRUMbO4fw%2BEZXfGjCIcseKUnHh1m1Ksr8gO4%2FsSUvIeKhTxPURrouyXqIUKgKrsB0l%2BMIny9cYGOqUBeWZxQWuOZZKwiws4JGPoJCdxn%2FtmOty%2BeW1GjOrVmA22kVrXRSEK6Movru0PwNgN6s5BSch3uuvfR0wcJmkF3zNj8Ukflzcw4ppHaXH9r7axyIP1ymOdUsbGuuyK9Y7ckMfBoru1r6%2BMPvyff9nEAfwfZssPIbHTWj8mV%2FQhrjbd4YpnHpC%2BVEEjyQmUQyVS%2BkTuOdWS0EK8bKDAp37y8xfd12yr&X-Amz-Signature=7479ab41055167826c13094e75f8162d2ff57b96834cd389aeaa42483714fd1b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

