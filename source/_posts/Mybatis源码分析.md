---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466754E2TLU%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T080048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIAUGyC3HGLF8MhAcP2TxzL3J8ongq1U%2FkP%2BCYJv0wRaCAiEA3bf59nfsuqM2fx%2FHSVPndULXf2qiSUJfeaCuENfSc0Aq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDL2FgBilS7tHOzbBPSrcA1sogG6WTrdk0I462B9%2F5JebI%2F4ga1NKo2kMSbIxegBkU1bQNL4kufXLdTvo8BAM8myzRIkvzB5tGprhaPhg%2FL1NmEybbvK6lE02Lmb4TaKzagPGZi41mZdD%2FlTOhBewZ2fO8dPPaNHTwrgli%2BNWK38pa01YjeXvCVyQdwz9hTN%2BO8N1tA7CoZZJCVzJtfqBX%2FmH54JyHVn6FSqfe31gSi6Mq3q19W6OPzQV7KbERGijctm9gpDAM0Tn%2BIfNJO%2FoRuwaBQBRqaxk%2Braig%2F3SespIWnGDDH7m0Co884iHpZUUzJv3gXI4YpZZVgj5cDJGJWhj%2F26NTcKtvb%2Fh6IdkuCoYN05DJrloxurykQ65VNZ5LVxTPJ%2FyYKnQxNjaI9%2BH0cdus6aRvYzAHgki6d0gFOP7Z99Fxgr2pBhA6CV2Cn9hZ7WZDamf2l8A9OB7RFnT8ypQ3259wcn66wU6dZix%2Bu4eFD083SXfayRzAAhkKVqchHWd7sS3e2Wz5axUJVvT26I%2Fg9UGY8FCKiswcMtEx%2Fl0AswAhsdEd4vwhQyUiR8gaBAPNabzxLXXDxAZwQF9GOE2fk%2Byime9AVnrMW0pVCL3AhNdDvX1W9aNT6qs%2BLd5gWiEXV7Kf2fgutU9MPzFrMcGOqUB2J1cW4No%2Fx7u%2B%2FoDay0%2FBaCycSBiY1rLR6tIeO6OloMcCKziY70hvkZlCXIQYmCBzmxn5oIqX7mAu%2FiQzL0UQUyi4p5f6i9YFtqLRdsk6nXkBWuNEFMWj2Slv6gd57zhu6kN2MVJFi8kvmsqFU3X2%2BNdiSwmHV%2FdccdlV6iLvcbFDQmmemAyJzHqLmqw4WnekonPL6gOHYnW6oVoxZFUzpMU4%2Bh1&X-Amz-Signature=c0aa5f80c92f71df034a8d0d1e4f2b93bb5a93cecb8bf2ae024421a4e3597553&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

