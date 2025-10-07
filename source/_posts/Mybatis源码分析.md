---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663JRYWWOR%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T030041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAMaCXVzLXdlc3QtMiJHMEUCIHOhbPQ1dQ4JpyxJG7vIB1s6JBcRYIa%2BlP%2B4VRljj3bXAiEAv2gr1BKbOPo5%2BaizadYur9YX0gmsgmKd8yq8QRQiWCQqiAQInP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBiiqd40hw3cVoVfPyrcAxo%2FhsFxKS1U3ZLU8nuFN7qX92Ya6Befuj4Fa0jjNWnBDWtSpS8YiqEYTy1DIF5vN9z531m39brFu85G6MNJlsiXfbKiDsI%2FzXJgf9HD%2FzlJgXy8Qetsa99dkXJopL7i3Yw6mnaM%2BIzXpSrt7SrmzYxUTUsjvk85WlOI2HW%2BUXVieW%2FwdNUBUo5wSKHgx2IrEopl33ieT6RjQN42rHiKeQZuttNUCoV7BBFk1vHm%2BAN8sksilh02u%2BuiAUkaHRtL%2BJmw%2BpcPvBrzAG1VwrAH2bgKlue56%2F35xP%2BMxPrdoRDoqArFKVhWc58oJTYIOWNF4QWfJYOlXW5UlZJKv7venBXm8BYkNy0ara0J%2FjRx4REMBjHio8FMyJGMDwV4WQPZoakaH6omyrKSw3ejU3NKypisaWhpvkycivwvhxzSmuEeV5uBAjmwAH%2B4Mb7YoCKhVEmGVFSyGjw8ruRxD1hZFDf9MWmMRIupHO1p8jV24QHommXRxMFGTrFdOvVIbkaQmXk8LVn6XZPJJLh6jKtWw1syiFPhSRMgDRK612OSd7I%2BI964vcdm6XZckC%2FsLDtOyQUtMzZ0GdtQby%2BlPhMTwtXvgT%2FrrNFKX9NDOxSVYOkiohEt1S%2F7wmjB88cbMIr2kccGOqUBAlIJNP6hz%2BatKrHaAf6kpjimIfnZIybFgU3hFpb5Zci%2Bol5d6FvRpRzXzr5nCuodflz4CEMGlD0qDA%2BX9VEVjCI68DoaIbaT0KL9nzylrKnL3vt%2FOCxNQaK3WMu39vjvzNGNZYWIP8OYeqRr8BlyNQefvfjQOBL34P9tmuyfBHnh%2FFg7a%2FnL5ZkAruM89dWq6N1JSvR%2F4vlWDa7fBMQZGU1Ibyfk&X-Amz-Signature=5d93f332652da0ddcb228f19df122c39f3bba6756a8089c8a0af7470009d0499&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

