---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XJLSJWCY%2F20250916%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250916T230042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJIMEYCIQDc%2Fflo00taVOfLDleI8gwWqz%2Btf2%2Fcteg4GWLvaAUMuAIhAK8OB61aWzTvVxUEFsiMTnv3uPtg14eoY4L%2BCwD7b50PKogECJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igw9NMgXchCsN1vsnA8q3AOWfaGAYhStN79GruO3DIYUQO6dRlzN3wCsSygwlJX7morTY%2BjWqoK%2FNEgm%2Fl4Tad95iPl9DgNF1ycYQfN02ye1ygjpp7SJWQF46f6ti1GVy8g64eqOH75Gwm%2BKujL9CLP9eOWO%2FZ2xBLNbNHhKM4CIP9fU%2FTRYIcCAH0jTNKaDtDnzuUZOGcSYaNnguWj5oxQvip%2FHgyJJKMwdoyHPk4Hwk46w%2FUQ8E0Da%2BICt%2B33C8x5pafmyxncJ36fLrAAuan0ILiiEJGUJmtFPffS2vZHJT3dTLVGMwrB2TjIeY2y%2FIdIKBgI5TBqvtUSwTqESUGBt%2FpKm8c7KBOv%2BFVFPQoQdA5mj3nMQ53%2BUitPbDWFKMB2%2B4s2U3zApjFNewqmOebSpqfGLEkwDSAps6Wt2bWoBeMZQ71%2B0d0tDdkgT3x0tA0p1mMipsbMg05%2B2QgzAU1P4vVHWuor8UOrdeN4SuVT9JnzK5l5NlQGxV9qrOX2bQB371iHQDTR8cYMVfHr0%2BIWBagG9pjBJJqPyP7RND9ZBJG56OY%2Ft%2B%2BwdQq9UL84FXuiIYAEGJPSoy2ltV%2FgN0FTFRC42Zvr%2Bxav8Nfjs9mWThkJNukJcp4SnaeONoJTGTJ%2BeugYvsidcGqlgRTD0vqfGBjqkAa93djRxCN9004MpC9Kcp8kZi6Ogjm8BEAgAIS4dK1bKu%2FALgXr4xugE2whKUypxBuEnkD3juVUeIVQQM1wHYYWvY3p593ekgkws7kLuxApLNKawMTQDUm0lpC2mcK8C8DA0Ls15r7ATVhgKhMeiMtEPO8ebZu%2BKZ2k7AmSEcXNXIsqE6kKuhpno7RV6cjH681yVCASvHMd9Az6DDo%2BZSXvRbrEi&X-Amz-Signature=3579fd9f96c72a118e837066808936a1642100fc7c25af3d2e4b8aeb8b2f0e31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

