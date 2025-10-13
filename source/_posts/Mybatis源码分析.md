---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TXFFI3L%2F20251013%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251013T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC%2Fui%2BKe2uKYFgl9HA%2BRPAj6BL1YRr8ucwgi3kgwB0iIgIgFkeVxBrEufx8axfRkS1IlnP4OVOZnKWsQ0vrjsY0Fccq%2FwMIPBAAGgw2Mzc0MjMxODM4MDUiDLFHUxAXV%2F0goo6b1ircA%2FvnrE1HALh0UnMsuRAxfH5HDDPJNgQ03ORrqTZP%2FaLXR1AWBLFKMjBcRnMwbHRi%2FJkWlct4NliiSK3GvtPA%2Fc7ZfsDl21uovOSrI%2Bju97q36VjKCOKjm86L%2FHpcL37%2FNImJrMOGe7iuB9nHxv0k4IxVQMmD82XIg4ei8qqF1rxp3ZJ7HBgjgZGrv2oTJgSdtoir0Ebk59J60GWT%2FyAKZ1rclsWQjmHftDI6PyNo5u%2FjLd22hMbFv8wTHAecvL9bppXl7i312UtXfhnQJbdj3b0EYDkFTz5F1U5EQr21KRAGTtUu%2FEzKByktNi5FaHuGZnV%2BkIHehcVzZv%2BrDL%2BaNfabH9AKvESUYUZDjHmONAlFrenxTGs0ZoJawJ%2FG%2BzPpHgz2XotZodKQm9Cyb3uyiQCjLvJv6yxUp3ov1SMNULPHDai0xu5kj3vE756dhfzJycv9wnRVPJAh6IcFMydRwNbmaiQZgOUm2rell8PtbAEBZ32bjPRspyl%2Fn3K8aQvsqiCtLGb6Cv5RbVxnNfOd1BsXot2jQfCIiYF7AmqOdIjHQpBwUtHl3kdXtTPGitJbQbnBanLEXt%2FD3h6%2FxU4IvvLohk0EVSFRDO2TLIMONR8lZSJ8PZ3Xcu4vw44eMKzUsccGOqUB47GnWbJzA0J4YqeKA1ZORLoQQI7pviZVA0KTdvq2OW2aG%2BGOgfD69x66QVk%2F9CxZB1TcWcDeJbDxAXe9PcQ%2FSCjoSSxIOY%2B0uCE7YwdqK%2FZJdUT%2FCTWbLVmXA9lH7kE%2FWJlH8ZmgqdLeo1poM13wz4TK6KFi0WRqhJ5BE2ptPdtpL36uwtwxjNnLCfo%2BsjxiG6GQj2RSBT5F9PV4kmYnCXYl2%2BM9&X-Amz-Signature=f0b5b31279c05f2e46206fdc78af732077c1abdfcadb02583a0321cd18d856ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

