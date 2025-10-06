---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YV7SBHP7%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T190038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCICmujF%2BGHHm3lOP5SW%2F2wp3Jf9L0F5c80rfAoyX%2BTd%2FuAiEA3rlTBW2U2y5Hm6D3lHEGpADi30iOXp%2BoMB%2B5MuCy5rMqiAQIlP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIydr%2BQtcqfF8LiOJCrcA7WEgkadO8sOI%2FC2OjTqo%2F2p%2BcQeKtgv2z2sNWqew0S1QD80cb2w2Qk0OCLw0w2wQpMZjwta2Ev%2Fhakj9pqOeuhMb%2FkXMBHrJuVMCndXaEaFnrwaouGd3r10YQIrhjIpKRKhdS948tg6i0xqtqM7dLYGYQXPn5obguGcjp053fP%2FEE%2FKrjS5qDwDCV6m3Rn8%2FNm8eU5D92sNHM2r1jAU85xGBUZcx%2B5tJfSaYk0jaSkHP%2Fhfg3elV%2FQV2rWrARw3PH1sJzNIViSjFcgYmQfWm1liRCI9LZ%2B%2BDnSi2GK3zRpwnvYPyk3cMkmLd5eqBT4WnHRzazhYV9rUuwgFzzsSpwUOjssZFlHlQ91zUi%2BOGHeQHwotOg5VOpCewXcMBdUpHI3aMpF%2FspGc76HGDJ7AXjRmQkQXCyNSKukf4d8r7yrJQEDutRTroJPo%2F10QW%2BjdlNgcRlFtRBr7o7JzSFqop0x029W0w1Q21aNu1ql%2FhT2QQqSVZulv872iAAW6yg4p6v%2BSAMnKnjwQoaPedUH4nM2nhLJ4qEtekFlhiWiBY1ainEhy9Vb4OMXDi4LmSH1CBiLVRE1EeErtgHm6UuTkzAdEspgrhJWASBCPVGz2XFECd%2BPj4i2Z1Ra2LiJ%2BMIiekMcGOqUBZbCoqvbSga8YuHO%2B9VhSVGwA%2BC0WZyiZwwnq3LPq%2Fh9DSNvNb4kp6JFVupCp1Pu40cxJPzJzTLqwPb2bfdshJVKkzobFZquh0cwCZ3AQ%2BJW1azucH44i2GH2afBkiTMZGSJC2W9PGooWNok1e0YlHROaSMphE%2BF6T0pdtSijjnk4fircsySvRnUXaJ4d5W3Z6iT%2B7E5Yp6Jebz0gOtOfLty%2BUix2&X-Amz-Signature=4d44be7a2755f6b99d73485ae4cf433aae9c5b24afcd340ac4182f4fc33d6f2e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

