---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SA5CNBHT%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T130043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCRRZIm%2BTQWkWWU%2BXJGAoK2Sb5EEFtacD0qi8bGsoJuHAIgIIVcpDlW%2BH9ePmO69U1v59ejHqmEIUKs3qjyk0DovFcqiAQIi%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPphmo2Mrvn3ZMfNvSrcA74QLQFKuxgjOjmv7OslC1lyZlgSSlm4%2BESXE1dI6iDrP2VJI7rE%2BZKARbLvUDD8YQJEbRlGSB1cduSsLmirffyZSBgnSBSPRKzx%2BEevGVjzS%2F3iZ7Bn%2BSHYsXUEhOqjjbztL4WygUfr2yb1%2FewKQvTx55CxqKBtA8mk1%2BpbBxpSqExk7lVO79r%2BelUxWNWsJKBofA8jAZ02CNhpNrKv4ozTXSmTvuu2kxoAMxYf4qlBAMKFS52ySpQpwe%2BF787kGEmTVXMzlMnNTomnDUXN4Fnl%2F2NET2%2BQnOm%2FQ3%2BvkrxCldCjXERYYC007O4UL2Ly5xddRM6Joma%2BofdNXksfLM%2FZbi3xdS0yTbnlxw7zE7BOL%2Fa1yAUnd%2Faav145WBU%2BvfLzecEsyn75wzCMNDM7hMelgw6cgR1BIlcrftPDesCKtH6zorkgQsklaIZVfveCP6XefgRU9IFcpNeDr%2BaVofyOG%2FgDo%2FO%2BhBvJUUF%2Fb2fWsQZaqdPSmZ1InNcObzubMpoI5L6N6PuXGojhVPoRm%2Ff6raJYkvPFGt969gjqfljH69HNoJ2WIPDzHRHKXhQmMbu9ZlRnxu1tt94BqRsiBG9AFNAVCrX4nQXLLJo%2BC3D41YGt%2FwW7XYmgCWSsMPuvjscGOqUB36IGWTfM7BtzhcO3acCwRD1KAY2NA9Is8QRkdnLCLPUpk7W5DlsSrH1wGyjvQUB18cICUXuhJyoMifhY1DG1M%2BaD4pXJElkzZRXiVfZpn%2Bm58s90FBSLJvxmR3xKqM8bEWZ32a%2FxymuIZOdCJ59weG1XumIpOcR2iUoQoUUyswWfrHoJmAVU7TywWj16S%2Fp%2FtVgj01e45EGQf15iZUkWMj3xWsUS&X-Amz-Signature=cfdb079f477bcb50d8d3e3e046c7d3292d481010b1c97378ab018f4dba7edf34&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

