---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664ET5XT3K%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEO3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFhNC%2FM3MtptwsNZvbcFBEl%2Bjyp2h%2FVxr%2BdmOrhE%2F%2FsmAiEAlY9e6u%2BMgADlyMGE4sYgWfooKyWb6au8OJR5xwvOmWoqiAQIhv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOkTGCkEAZ1bX0mU0ircA2SHsH9h0XAPY00r1o4Ftl2QbZ5UGxc7U%2BQt3nRAoX2MqfSloZzelEmSlenOKxuJqv2djrdfvDr14vdfWz0cL%2BV8xcRrrFtPkBmrGUBH%2B8bk87%2BHk7SIU%2BGF0ciOHpMW6GggILchmKO0N2BLAlVRUpzZcvAyxx%2Fo9iaxcrNRitXmdsCL9n%2FjkZ0XC2fWRscwroYD9eBaHJaVrD91Tj5W8UhcB3WTXnu%2B%2FoXWorRf9toLx%2FGhcjE63x25L%2BUK9Ga1ulPnTGgsL9LDVoGm%2FmD1cyrO%2F4r6Ao2Q8l%2FiH25ER6w7NtyaTa6v58bymopRzFQjjl9%2BlJcYO1s5wcGQiaa%2BpB5hO%2B2kr%2FKla3YxYauBwcEDsVQMLde%2BFnER3OeBfJvRxzYeYMMetr9STLNWrTHX97mVx7PctkA7xRatxx3UV7Z6KYPtuXccX5Q%2Fl%2Fx%2Bd5mhLh00wNOAnueEjnNAZYvCL4qZp%2BjA7ahh8DlHfrJSvemSyl4pDAN1SMScBh6%2B4sw2BaBfkCtInrDoetZ2YZaDCZCmwtEa1xibxETYl3KQzxI02ZmaEWtR90M40E3mgWO4z%2BvuQx0x%2BxzklZXe7fOrwTppPsSF8XWvCIa5%2FReu7C2zgMz7iVh%2FDHobQ6U2MK%2BOjccGOqUBAwwBV1mGRNCppw0nKn0xchPHqYsjfR6VejOYiWo%2FZ7oe%2B4WuGMJAfPtmE0zaqnj9tRxfY1qtb0mkcsbtC2FGatUdT%2FKIBedlszsG7XCPkto7sr4fz4kckNhqfT7d%2BH998d0YGRR4Is5MuQHuAFMCxTWSsHZbZYnhBnkvLX5XvTgoGkSCmC5gC4hND2ujttrMo5p%2Fp93bcUqoPPknWRfVtIhAMXBX&X-Amz-Signature=cf9d30cb58a2c1fccb8c7fc2883df38090c2d4f2b5be5ba5aba81c3f6ce2cf12&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

