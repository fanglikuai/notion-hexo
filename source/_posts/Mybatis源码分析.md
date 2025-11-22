---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UFQZEKPM%2F20251122%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251122T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJHMEUCIGjm8Wa7I5ek%2BmxoCK1cmxF1HHH%2FRRTFihnRqf4eNkWwAiEApm5eNzUCLqn5YLDQcuGuwCLrwPqZ5OHGSyB6z7OzqfQq%2FwMIGRAAGgw2Mzc0MjMxODM4MDUiDEv0pPasOzjH1w4qHSrcA6rRfjTKRheifnBSdgUW2XNRAT8uKBVpy%2Bz2GQVCx66%2FBfilSFzFBd0gXiFL3ERlS03PzWelq0mCtiTMXDikk4PKMLch1vjeR%2F%2FrGbBUgqPfGCXsbWf2wiE0oHG9OmWx%2BwmCDnSfnAudjOJL%2BtItLBn3Qrin1piVvXQnQK7bHck0UyzAC5wbduqutgrieUGUu6Quve5CRJHHwsPPbFbTSUsEsYsF2u%2BeCp%2FJ0BHpjuL%2Bz0ZTo%2B1EfJc9hlI%2FNmTc%2FyCZq6qE7GZ8Sl9%2BJjEoPj1XLYfu3eLrs1Vtu9typjNJTsyWPc6J4i5Yyh4pAmZZKb8A3%2BJ9mFgUCKSw0BBurAg%2Fgp6jOQFFp3GA1wnnv%2FNPVdJibS%2FWxg4NOr1sGz7KG%2BIv9Ezeg7b%2FmGlDxAV5ucvCbT6TH0J7yXD2yW%2Fb7M8%2BGSKVcYzfCQ9Ro1obAjMIClopUWpw21qamp%2FYbSiF1r5TCOCBVcpptjcJjkJapu4w7BVHsa9MyNOMyxFy1UZxGOXgjNslJqTeN1CX6TU42Sk09PEzYiGrqVTanECxNEobtLiTSFCHMMsSf0%2Bct9V7jS6pilndv0q1Vhkq2ppWY%2FcHRdYpOG79u%2FgACzvYpS23S0WLtQF9IHfHzcIzMN%2Ftg8kGOqUB9gDxE%2BTkB3m1XJGQTIMd%2FBkvmj%2FTYK2JPC7tUCxbS8ZR1%2BXiJqahKwFxffB4uYUs3cz2y1yELVzkr4lg5Jc4JHBZHian7%2BMGY6RixdHJO51B4xSWpAu2ZO5o%2BopTthPCEugc3JbGY4bhOHaLTcAp%2B1X6Wtvm1IvPzZL2%2FyMGpm6zQdnlL7Cz5cxYlvO2%2Fp685S%2FlDC13HglDcQddONDj%2FMJsc53f&X-Amz-Signature=b49c8bdb359a3492446513ca3f02e8abfa2bd053ed25060782cd78e1bda9ab9d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

