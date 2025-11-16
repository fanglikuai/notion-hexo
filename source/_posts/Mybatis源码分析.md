---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TFF7J42H%2F20251116%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251116T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHht%2Bb1aHsnqFKS6s6KdezAinsFJJ6h0H8o%2B%2BcNLCae5AiEAiIuqsX5UNSMIQtuHXpzOfZh3CLpHjuyMENMZnPeQcJ8qiAQIkf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDJoFZf%2FbrVBJcmNISrcA7BDoahCiwB3Y03pLR1lJCms36z74JnyWpzNcxPWc%2FuMYNzY%2BoAQw5MxY98KWRvKyWyKFpyz%2Fg4jkyncTCZUKwI0J79Qjs%2Fmg2LbsshgILNxbb14npYfhh5UzeJrhQO6KXI7v4vBuKTf2VGQ1Rp%2Fo2jlyo7eHYGUmypldnGOfezfDhq%2BJ5ioP%2Fomd%2BtYmeazPNG54GnC2i2SWV73N6HT9XrIBJUCip4xiiL0sHKBE03MeaT2mYos8NOHfeACOMnt89ckWR%2FgLXNKW5EWZ%2FDUwAg3OUNmKORYU5xQtJmX4Ro98uerTR1PvXdATjxB%2FZrKlHweeCAEE64q9VGB0YXBRG2BKJWiWP9ZWU6HavfIaxNQgCcsmfOlMrSDd1j%2B%2BNozjDh%2BiX3TZOcbRqVKM%2F57jBnFqhEWtVngIzReuvsb0Vjbhu8bmkGn3u7GzsQxugL%2FUUFJO2%2BdRjOCqU1lTyIwVSgu74lOCk5dWtqtxF5%2BcbVbE2VTmgxnLIkCRSJQSbSI0QieCHqPCThK0TOLJ1%2BwbJrXTYt3LEF%2FmbKaMHF%2BCpIgcO4lLB493i2C7iwuzWrTDWRh9EDLVe4V7aP05tALt1INxgXcJGZlpBzpWZAGl2dhB3Jppivy2vrEIUQ8MMT95cgGOqUBs8cqe6C%2FS4I4MKXcSvEbyWRAmR32nwo%2BA1KtJeme7XwMAlPThHSYgfOoHJFJY5%2BgYoiF%2BhO35%2FA9An2J2OJO8Qjrj0gZXo36Yw%2ByYrv7UU2Kk4sRmMcI%2FVeeE1c4olr18k6yy78gjYyGekHjXKEyH71UhjKj263Il6Hnva3RIW7KYkgXsQFKe0iQNO5oEmFz1favomFVypNp%2BbWd8BOI3AAL63ua&X-Amz-Signature=4531449fd2131168337d6a498f4bc11063962e5100f9cb0da05adb4a4ff1466d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

