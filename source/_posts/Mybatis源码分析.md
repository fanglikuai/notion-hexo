---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IRU6RCC%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T070050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEYfRW2IE6W5w6drL5Wm7w98%2BOH6qPrNSdEblHU08GEgIgJSKX8IuWVEbZx5ah54p5IShoN04Mu27o3Vk9kEA4XcUq%2FwMIUBAAGgw2Mzc0MjMxODM4MDUiDIXK5Szpns%2FUDwrtxSrcA9TcyZEe5J6zRDa%2BzDvXFrYlxr6Uw9uKs6Tcdvr68mLJdJJT%2FJBKAE%2BO3Unk4Jpf%2Brp5nn91nfsQlU%2FGnVyZPY%2FFgJwxyb3ElyUhFrR3cl51ULuhMkD6SslGpYJH1tx79AfRIdXc%2BiMSYiJPlbpAydqKab0OP9d71DpEciNzwQw%2BFGIHABFv5iKd%2BZbtU4tlbDEADxbU35m%2FTvC6RbHgh5WGyC1g93T8J9ZKyX4bk9ftbPPSxpAddAPGNFHtOmQg0FJyqAiP8a1tVmIrJ9BoIgr6SyVu9Dm113O83Y9rJS54DD3MLBtpzeZui6mgdECi%2Fmcu9QrtsgwkD5VTejc8xtB3hFRSv5UgXHivCDT7AB7FU2G9tMMw6FVwFXupmAqyuSa6rXhOPyrg%2BqixJnQ13GZbA5%2B0z%2FQO%2FtYkn%2B9BLE%2BOHwRqsMz02Dh%2FbbfQ2ZQhHpB%2BaNjpCn0SAZ4vW3ZinV%2BzUuHrbgInei4gpai2e0LZSkRtFWPO5qEHsZ3z7EOyCduJk71Gz33gRFXPPNE2%2BXwshkKETttLFlBcgUGMVdMJykTeEsRPwADqVA8O3BrRKqcabHCd%2BhR54naeOvZ6nKjXR0x8WX25u8n6oofvtvxYIWPPa41lFsJnyWBoMMKEkMkGOqUBf4BBvHqXe3N1HBFO1Gsc%2FyZnd0wHi6UPDFWKEqBLfma9hW0EtP6ybjQPo13vyQk6uTe41gmQtwjQfxYRqdDO1MBt3QYQSAVaeLsJ3iMzaWFV5vfZW2ehKkCtJjJ%2B65Ww4uz%2FKWZO17Fnxy1kPHIjOna9g7MBC8VInlFzLtovF2F69YUMXMjF0uC1JnsTq%2BwNvgC25QMu1coh0caSq2II%2BCOlvPuD&X-Amz-Signature=e17fcab277694f05089a589f05654606369a9c3730cc79751b918e0fb26f2e43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

