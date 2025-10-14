---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666GLPEE7B%2F20251014%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251014T010103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAhawnVveyr5wQpRaoWw2k7Oyo7VRLRbk2qNBQ6uINw7AiEA9QyZz%2FagNvAIlH9MgK1Fy%2FeQ%2FLCwsbenVjMUQCpC7lwq%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDKuY7%2FKUahs1puzu7CrcAxyP6C8ol0%2F%2BH6gq%2ButPyOPuILcqZkrKoHFTVrhmBl%2BQ%2BVd627Y9vzjFlCIzFSiUNpI0O3S8%2FVPavtMLjHRo%2BVodEVRjPY0zp5QUWycI4dUn5RysbSKUVIYkIcRl9xrhc6r1zo9gxIvZaQSAxUK29nRLdAaGaBemRoXYNG0k1eZsMZGgcreKd2EPT%2Bnxy%2Be0ElRP%2BskhOtXEnavhiwr6PSvnRZ1AtH57SpgUZik2HERUXJENmdEyPwJR1barTpZkusX03UekpszVZq8W7eG7Bg9Tt7zIGNihp0X2XFAndoHbxNx%2FLZrH1PzLARCTGaQX%2Bh8anr7ROkC90rT1usooiRgwXTsr9soL6GNb%2Fgy2j7AYuOCpGFPsjeol9EY4D23DrqjO5km3o7yudBGdgpHD1GWrp8vWTooLfFp2W0rPcLDKvvdGO0bn2gyoIN19hJokpl0lGm%2BgHx%2B87n4bQ04SZFoW%2FcsUXa1RCq7eEMqkW1pVraHL54i8uO6sFGLLbZiU1I2VOQhyMUfOwC2nJqpSNxu8HPRLVwjzJKAp5o%2F7EXlgSMVoDnaLG0Yb8tJtJP%2BfdfZ9LCJlEWsuUKNYMRHXK5xT7unVSYlNkzlHdrwvo0sBwWGEHFO16SBveC59MJK3tscGOqUBrRSZSgmyuuXcZ7T8g4wYHJVqwD2p99cIIugX8DtA3ysM49EcNmQ1wTvZTL3NyjAenFTvwZ1aRSuI4bvYP1Mgvemk2u2BROtsNyuTYUSsU9J7%2Fln%2BE%2F%2FTBiDIu61dtOvOiEdZ5xA0ptMOJo6kjJArISGiIhqeNTpngwgpG7soMOPTUbYSMCylsxL3YiXpYjSQkr5PbiOYVPgpm3UbtBCQ3WrXIUjS&X-Amz-Signature=e9cafa0bfb18624acf34d024a56fb559880ebcbfd093ed892296ee1c2eb89bbf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

