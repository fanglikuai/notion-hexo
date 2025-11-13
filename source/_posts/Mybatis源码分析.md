---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QHDWAQDP%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T010039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHkaCXVzLXdlc3QtMiJGMEQCIEpgg0KmTVrHUn49wCGDi4ckxgZ0Qpmd1XHvLrJDzrmKAiBLqKdj%2F3gLwyY6aFdd%2BQxqNqrx3kkwmU8bMrhwW4fp5yr%2FAwhCEAAaDDYzNzQyMzE4MzgwNSIMNtt%2FaSBDwn45Zks9KtwDqvjLGzYYuiMtrbGsQRYMJtpkbmNHrpttM7woOX%2FcugVrp5FzGKMwQdn9KFnSY%2FNXSfIUZr25BH9JkQ3IT%2BJqZ4JlUUapNBc%2FNvldR9WU6cyYXBv4vzOipKRv8YoWfDL8cbj45s7dbDu2CgA9PDUBRZQ70TI%2BFJk7RI9%2FxaA0GNoXwk%2Fb6eHUu2NQbNnJ5JjYBPAYjmfUZYraNUmgT6t28OpG%2BIysZFzVObOz5qtY1xUVDmZKoSsVfVvLJ74G5Cha1L%2BCbjHf%2BdLtUL2xcr5zor27W1r%2BoZEZV%2BLS%2B5yE%2F2BNiquVNyMYBRr%2FfSCqfgp%2BQ5Gscfp7g3KeBf0Jif4UKlAhgB79u9cQngeymnf76JWc0t7XE0jSKEGI5%2FCUiaRAukYwlQYgln59PCHZalMGqZTDu47Gqo4FPQBvx4%2F3eHs4jH4HrFPahKhcTq8JyIM4zSl%2FHjUGlVFL0MEOjk6NEwsz0UdfbCn0eR4i201C7opcVor3yoa02P78E%2BjkHACjqM4SJcJGvMcVfZYljZQchgpxgItQBnWvnqlMZU3VE9gP%2BpKlUiKQYSB%2FFAG%2BLoqSKvgIi%2BN%2BS8bL7Jiw15NtiNyFEkXX2AIKMGUaoIJa4DWn3oKRoafUdiNNxm4w69nUyAY6pgEL%2B9R5m3XImowJIydFRoT%2BmRSrNX0XhhKY6k5n5bFSRWoBjHI%2BtUiwdvNNm4gFEOs%2BgUAJp3JR4GfA3zXi67OOk2%2Fm5UGAjreHyZJbE2wSHLu75CKyGQfyURXBPnJ00WTt7TbdpkE0mgAnqMsVeLgOxtyiIok4r%2FvungAIQa8GP%2FTFpKZadfl9xtxuInJ3L7XSEUReXpabcTNWrDRrQ8lEmzPkKF16&X-Amz-Signature=92b4fc6cbd1373132c642ec025e8d72bcfe56eb79ba695ce1277af36e980f275&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

