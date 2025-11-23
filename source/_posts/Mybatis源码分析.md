---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SKPAF6EM%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGgaCXVzLXdlc3QtMiJHMEUCIQCciAv59mcTxFk5dUv6jJKyJa6jVekr%2B721PN4iskijvgIgS1OknThCM8g79JUtNUohrhstz7U86BHsi7droaz2cfwq%2FwMIMRAAGgw2Mzc0MjMxODM4MDUiDIg7Ma2s43U8dIGVrSrcA7QCQV4RlKaahyUQyJxB%2B%2B5dyZQ9QpIco9PGGjb59509tC0WD2WIAAHzkc%2BS1r1O0%2BDaAL%2B%2FNFCDn248EI5dRgVI%2Bm0EQB9nRhSYivtpC%2FPoALwFJONadeqIDykh2Y%2FQyWZtJfCzXIfRJpbqcBmUW0zGGWLWmX8zOrGZJ6oNRc7tqh4K1Pbh%2FEs%2Bjjz59QaGH4SJX%2FNRQjQqbC88qXQI1JGVnDl5s1f28zTQQY2pNj%2FsP13B6I0EyP70Hw6f%2BF05FPA0aykxaPBEhF9kzdZXrUMzF7DMHcKIBRYrt04MKdOrAOIblVBhy4PwAZFGUoj6brzII2H%2BkgqWebzf2Mnyve68ym6ARLbQ2PGV22G1jMi4QGcPANAogkifJ%2BJWhTFJSvVS0ykYG48SJsUAzVdkqEZwNOzQUmrstF5p17kcQrG%2FVp3EnfvU1sMVIlaKQVgEURi%2BLX5n34bBvsurKsRgDAF5Z91tMfyW3rHEiV8DQJprSwHA436XXKP3sxPjEPytwrsmtwpe9P3Uj%2BESZkNC2A%2F8ph%2FaMItUVPvLIUVHx6aoxKwLbrWkYCkSPHzXWW3lATgQQnai5PjoVM5gtFMokoFbUBb83T7Zr5jNgH6aVUQu03Y00e1AJ2j8REbSMKOfickGOqUBhNTx9fBerM1B0dPpQarrnYxp9ZmaJU3ufrq1cKMUGvNj14v13STOKpi6EtwwdKs2AaAKRozoBVVsaZoWJ7nLR2R4dDn%2Fubwlrds1bxFoG9uuH%2Fzph2e7PK%2F8SwTalqI8aGNHBSJnqE%2F1ysAfgE3ZyjllhG%2F7879F%2F9H5WlAwpf1h%2FDD4sNGvTkrw%2FEMUZiPQxp2NoZLl6ZA8hECS52rN7qnEjKgx&X-Amz-Signature=367320ef0eff434ede405fee5a6e98e12f581ebedda8f7b43b1ddbb776a14910&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

