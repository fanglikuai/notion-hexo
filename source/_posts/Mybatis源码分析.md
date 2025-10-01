---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZTFGAQBQ%2F20251001%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251001T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHgaCXVzLXdlc3QtMiJHMEUCID2xhC04ggTQn3XDMGCS0LYVj0o0vn2ERFZ5wEW3EeyUAiEAhE2pnG1518iJNJfbgYufta2gOvTJKVnXkxJi2P23lAwq%2FwMIERAAGgw2Mzc0MjMxODM4MDUiDHpK2239eMCCLUZAgSrcAwJLMkWCbdR9Fnabbh9Y8Bq5XCDQWpmY1BRzYRF%2BGmH%2FQRIsmpxEFR7tFoVmo0R9nwLBBpjxHNR1hEyX%2FLq5iOGuq%2BH3HN52%2BQpielqwkkFjFKGpQfNFG%2FfzuE4vli2wk%2F3IG0wPFMKjg%2B8DmNy91ZoKkd%2FZVkE%2BxByn8IgU%2Bv8KNAL78%2F6%2FImNRjVE4QMn3hJbfhVQK3pUYHV9AFCcgjLYzkP95lXuh2VdbyDjwSdMwwNmRRR4gBe87voRPdJsYp7X5%2Fv9vhEzzF1ym9c3O1kqf7FhYVNQS5oXhae%2BtHB2SbSKTSRgBPc7uAkCmxosNcB9TVN3E2sSyuTahDrrlnyHczhHbokmu43xK07e2%2F6eDZuQP3p%2FjUtf0Ea096jxWd0%2FQAuRqTuQkcd4lUjc%2BdqYI4g5RPyqcySa6HRoW9o4sbXXicdpGZcwfHLgN6IvXviS4zPDbWOpHJbEkpCPpJI9GrBXIix1RTyLczgw74aAf%2F5ohauvBTA6rwqxr2TGJk01DjMJxl6g46EhAzbhdyEeBRtyxMMH7ufv%2BNI%2Fuf1Lpr0SKyYrpwcTyu9zR2RYPgxkPrTa7snGXglnJad83pLJLXeMSy21sTwbK6b08WLzz9WSBzG%2BYGRZNm%2Bk3MOSy88YGOqUBrfrrG%2FA4rR1Nf2MP1TLjRpY2DHV1LOXdc7gVEPWjFcJku1px08M6fnS3zNAHVG3RBZc0e2iELV9zvxjbyRZUojrLTKglsscgwcMJ3Hqo%2BJR1Cjd7bTOrcRf%2FhDNEWDR7KMCsujnbpYa5ea8KL1YmR4PLA1TCtp473WMlRrcWC6QVDcbRYNtwtSbc9faF4nIWt2hU9RevFi03PD2fS9pyFhCh0E7J&X-Amz-Signature=9e51ce907b431576346457eebbfbd0a2ce3040a222025bc1b4dd39404af02fb4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

