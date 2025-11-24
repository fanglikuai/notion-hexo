---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666OIEGWKO%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T030039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIEGGtFBXKorbQiXWqdErc6M8OPT52HJALqXAv6iqc%2BdMAiEAvfHzI6kc3DC0fhWSeGXlAQ5r%2BtIxe3szPVmQfRdoEXoq%2FwMITBAAGgw2Mzc0MjMxODM4MDUiDOnwl3fk82p8L2xyzCrcA3L1bfiKFSpOXgdqd%2FC0Mw5juoJ%2BAXEsgsCOibf5nTBl%2B%2BOJL83h9v8AmsvRCTRHTTO9cMpi8zixb3leeYbSYDkPTmSsDPhcpvP557Cfm%2FjwGgwbWnEPyZC3vZWfF53PG%2BComuc3QLjh%2FPYGtYyMYZKRwuA5kuAbWX3iG5Qld929FTxUXV1vN5NOyuvoKsPFYW%2FFpcuZhtttlNyDPjwOejP9WNHIkr3ovzmV19HSEKnBxsGhfO6nSs4r80ei1A%2FXEoDAY4LBkvUWa4cC7kHG16lejSoPgzAJOstjUg3UCUwMGtb3k5a7DsXtIBmSdDSNb6FNPtBaJ8UTeLHxiWUprZdD881fDG6xHm2O6X%2FL0t8nf%2B8r3g%2FHM4MQ6h4hf3B6aK31HVOG4L3txAGa2TJfTBnKz%2F3%2BTQk1m0LWke84vHrXjrVWPkM2HExOTPKMgqheSzy7rBog9bfZnXt%2B86qUtaRdXBCYLWdSNSTAnxiL%2BJfTlCkWLs1kpWAEz9s8ryU70ixogr78IjoO%2FmJvMuCDlGmXUvV8Fb5V1%2FK7AMPILQ70KKMaO99YGjCmKDhhOQJT8hHugdBcI5vMQo0nv%2FT0CM%2FD%2FdSQLwYqgdxnLxUiNRoAAgaa4K6gFgvqhDUCMN2Oj8kGOqUB5UGb24vy9GPq0KKQ9RYANBsiJb8E2a3OQvmSCLfn5M9VatLdtGssOsBd8B8XJbYy4TuEbRipG6fl3jlZoeAwGxsDpFbLfCgsyuaI%2FnW6g4A34IFCrnNAQqrS9NiGwIVuLBkvoKwrblIYqyP63xiAGyK8qOQ%2Fx0%2BpI2mSMkjatmd3u8%2FKM1GOfLlXd9ydWsMigOOt%2BHwBTcuTSrFCClt8iVPPB5vu&X-Amz-Signature=ae1d37fea5a26a27c8cf42880590435ac46cc6c0906e2865f7b4f6db196b599b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

