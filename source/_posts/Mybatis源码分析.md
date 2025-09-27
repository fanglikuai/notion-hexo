---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EZRVJ6P%2F20250927%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250927T040047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIFpFOkSjjBx8D1Y89ubEMHrDnRzsqHbUnExq15IvRoEEAiEAyb37Nk8UtgIRoo7cSn5LxZSTGUtL3Zq16EbiPmLuKKQqiAQInf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDAshf00XvrXAAIstqCrcA7Rfn3ctEZCPDf0InczUr2nE9TpkFy%2BaI5kQITKtX9E5GGwIVaw%2F7Nu048c7SETowgcKdbrtOCcAfBptetwpExJd7s9dortCWQ07jqd%2FdCH4iMJMC4810V3LDAMOoEbDPlWR9PAu7Lh%2FfFomhFBp%2B855DNTY09CXaV2DPk8ovbUr3r1e27oiLRtO3PamYPWy%2BkKrsUNw5PZXlAnN4hTBQQI7kBSPP1pxIxW4SnJby0IqY9zQaEiZhIkGvf%2Bb2%2BZKNulSPViGtUV%2Ft4Rzfrg3Blw47cIb9ifmsuM87RtvzgaLc0sOgMSygExn0lMQYSXnOo7gR3BEdfGWe51T2wdHvFsandTd%2FgNeaY799BB3OeCBp%2Bh2yR4h7kqmiWgImgV3G5zJB0geoxus61N%2FFQFGqxJ%2BRuy96IezPMlSeEBToXdnLfyL9tyXjZqvoeGawdMDwvh6x28Jd%2F7ioGvf%2FCXWpOqId7P2UOUDPAyNCXkC9ushlB2qETWY%2BRr5C1MQ2OxITnEPjQYJdh2WsmKL32VKY6erWCWJiY5BrFX6mk7RuSDi8dZ9JYjaQJ%2B8238d2WvPHBIgN0gA5FVPy2i2stB%2F9oDnQNp%2Bw84uscYYVBgouHZy7OG%2FQs7QgZR%2B6aMIMNG83cYGOqUBWPFK2H1zTtIosTLgGBvjr%2B3c27HKQ2%2F0av8eQB6mYQx%2FakLRf2bcMx%2F887BAqxeqVWSBjgym9OeWIjg8rKN3zaeqMmxapaE1YLTqbQU16I9alq6LIQxlHh7rMRGEVAipxYbhnNlcXhu1u%2Bey2e1k9hpAQhiPiIrqK9Sh41xqgZRvfSZRhHc%2BuWgFLd0uiK%2BwiU4IgxTYtq1heTy8LO%2FV7s9T96Qb&X-Amz-Signature=d6a86497e422c017e552948cdd0d913407709a4cf42b235d6ee7441931e249bd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

