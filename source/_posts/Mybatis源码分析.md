---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCMDPTA6%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T170049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC17y1LfvcEJ91Ek%2FzAKcJ1fSf5xDVoRwMQhoZuglydNgIgSqx4RpzfGrTY89TZtYJUaHiLQqWeWsm4MUJmDDCqkmQqiAQIqP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBV8KXmER%2F2J128nqSrcA%2FenPiCueQoFJ4HPom%2FarDB3NUEqDjeyTLkwF4qK5CIy0a%2Bhsfn2Vmd1szOs2RUm53lJCJVVqJtb5MEKf6aEzVKZyGeYXLPV%2BuIi918DMYhf%2BaKLCQWp4uQe2hwE8xIZrQpap%2FifhluaDsk6SXiFAPODL8cf6xmCDNToItnl5CvifHD8i4nqD8DKJ4u1pu0dbhuk2U0K81zC5RvlnJRtXV%2BRjFFn53yKBX1tkSIBLVnXCAcuuoyuvz7M1xBfZ5w0SO1dBd3RwAXq8FmAHwAzfBwU1IWdHmv%2FhlFKScIhOyjBtVJtMxXDP9IQ3mjQdg3QBgR1rid255AGN%2BPP0tVEYqCEbZGwO2fLA4kOdFpOL1LXO%2FrWfMbXr7atL8BWSvwadLAUM47VmFXBYG%2ByTAP4R4%2BWOIL3Pq3fqp9OmK6qR7RPiEn1zxF%2BZaaGFFSkYFK28rX0vkg9XF%2BbRzl4DfSSz8P%2ByfRWD5daYAPID9G9ED35HC3uucpRx%2FudF1KAWPT49FYaCjeg7F%2BGTZ0ZIgORRdREj8NX3OKasRK%2F5YbbyHcYnBEhgkENzGcXbOJa%2FzBA2WOJTy2LXU%2FG5vRHmZIB5lsfP5AaxOjPqpOeZpkBw6Kxd1K2L7Z%2F83K3t8LhMMa2yccGOqUBH2E86OpfMHXkFj01abGPhIKQulTjP8DSiKLZttFS29M6o4Ts5xeUgP01m3ui334qZIpzLiondMRBPq5ea0maI4xbDS8s1xQ5h73khGoLQMAhv6pdPwQl9GusIHMN9KXB4QciHKTHMVjhcxB%2F79xmzE4KWEM3Njn77%2F72eW9SBIQHOTSWM0QKUMKI22SKhiH0Ot8Su9dhXVa%2FJu%2Fvyqb5WHFRjXxn&X-Amz-Signature=650cd549f9d043ab2c097e4cb06031113a9e911adcf7209b51f647e226acc2b3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

