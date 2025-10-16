---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664LVP3YP5%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T110048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDBCJm2d2L0%2FZm5%2BqOMevPMb%2BkHVps4l7z5WeLF17qr4AiEAtbJ9TCyVxh2SNYkCods2gt7Ccy7TNkHXiv3tBqqGr3kqiAQIiv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJohYy7YV9lgedFXryrcA05sNCO4B7lI3t%2FA%2BeCPhJMNCnKYUy%2FGHcdr1LDYXJIL2fZyuz5sxVIHugp1eqxQtX5skGrsVPleg58dEKXRlwTOpZfLZKBupij%2FgcTpiaBOsji807nnPcbPz3zwkGuQkBxH7cAAdfdDEHfnaR7BDNAMTgq3nnoWD73ED6W3J88HkKCt8LgUh33LcEUr6KHOReWaP7GMK09ZQ3SykqJkbMzcZ7lzIxlY7zeu0mZooQMSMV0oCj8NBRwEdqfWVsO4Zdm5I86hK3A8XdqPAmhWkCDLVt3%2FCBxJKQvktRY5A%2F%2BVRuzLtQmzBNJ1i2QZN55Mli%2FNBOD%2Fs%2FcC9WrYdXzPH1a5rakV7S9mrd3Aq%2FzFxc055S1zXqRdX%2B8N%2FLGUsuIIpG36qXVLgbuuEhaUU2QUXoGkKo3YVZgjszWSja99vlOUOxdav3eeKSYYL2bWqVLYB2TNxd%2FoftRFTfiyvnlIO2G0vow8SYWy2vbMW%2BODNYGXUZaz0Qn7Oz%2FOIyLoWomMhEBuYJSQkCU%2FXs5VvWxyqrCmBp8xRpADfFQBIWpyHjhgJIoz8jQy%2B%2FSSocRUpoHnzmPD9zAbt7j%2BRb7tX0npNbuYzbq0Qyv%2BgUoxUCs2gozDuhkY22Edi2ADJczqMKXawscGOqUBdGsi%2FWvFKWd3N0yYDuBvRpDLr4Il5MzjINBqzGvI%2FlPOdKwSu0nLOpxC3bN11n5oEY7MmLO3N%2FsYX%2BM7%2BdvWNHqjvpdIpCTX3n9tcb%2B1r2FeVETolG%2FzF73uuwihjwl6lagTawsWOEqA9gp%2FugEXInPo5goZlce%2BYH1U7CfqeksX%2FApNOsgl0sttErtMD1T62aQM2nUaI9atva5V%2B9IuOqndKyMi&X-Amz-Signature=0e570f485583ae5912b9a5badc6709927e14dadd8aa020041aff98658bf48462&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

