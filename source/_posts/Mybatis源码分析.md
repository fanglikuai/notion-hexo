---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665IUX2K6E%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T030047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECMaCXVzLXdlc3QtMiJGMEQCIFA4PuHoDpYg76SE6qdX8wXmKJEJhrgZWgNwrSAQd%2FlVAiBXNBHq5HbLv7KM6JNHGz6H3rMeebOGJej%2BFsSZpMCk2CqIBAib%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMTtfGFokEju9vrnT5KtwD2EDLy9aMX93HcOGbKju9UphXMsV54xMOLwBQX%2FMZPa6WnFrJac9fvGcljdsK9H08NE%2BHEvJFr%2F2UQZVbFIE6sWm%2Fzyn0m4sDllWl5RFFNOnwJW5E4pPiCT1OKT3UUm%2BXAizwWuzN%2Bwia94adLBpgih4UjHolBE26AMBmKnVyAOXIXJNgh95nziokBh7lPOn%2FypzYLY6Eq0ffC9xgC7UQZjaABtw9cDrqKIkoFvCZrRS%2FjUuZvoPZtmb0A3E6QJjKH0WKICQgMluaSIWeYpsoeDO%2Fbx46Oe%2FXqNfDpSRYWVG2WJHl2ydKK5CC%2BkNkGSdXchjXgChWM%2BNyD66tZJn0P3co5S%2FQDNuaZImFh3uu1%2BS1Rk0sSqTmhI%2BoOPHKNQ1vcgSUAdOz11%2B4K3%2FdRnLuGcM7oA9h1fEOGigm94PpaOavyGu%2BWpVD%2F6Z9G61Hsa1LoI8a%2FWS0xxJULRmPLUp3ln%2BllLOurB99RjewA1WomIrup7RnwMkLUKtAzs3GWZGTL7kfJG79rJ37GOtINeUr1VyaWP6KGl5zCMIcjQ9TdIYLkk3hghfaA2eC%2FFAR2f5zHzW4LP%2BPFSp%2BE9bp1uUbrREe9c%2FFPqMehkFStrjFUFissBVW1YqA91V%2BVmUw2rSoxgY6pgEBcPD2PZ%2BHWosQyDH7qOJNotXDhxiWSM9V%2B4oQm6bwQZw%2FqGGj9spMnc%2FLzzzLekNhRZMAycDImibJNH%2FUvxNrrX9DbCptfOmQVehvalbAEJERoeGZJlGyib7iPsUTz1DpNNm55sjvDTXf7VEa0FD8UKFqFoREJvmquVtGaD5oBPXhkpfKtcbSkw2TGJDOxajy0Tbtcv1u%2FZvvAYBRyMfj4CUD7oJT&X-Amz-Signature=904f49e1980afe829906dc5a71653e3f63a7a411511c7ec0ba9011e0d53224c2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

