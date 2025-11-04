---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RTIVALW6%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIBjZ4%2Fp5Agvt680hABeAnyv9nqn4C1ge5taYkOfMSlv9AiEA3VgtaZ3%2BqqgT%2BPkNV3QI3BXO1Dp8CH8TlrSN7PeRZyYq%2FwMIfxAAGgw2Mzc0MjMxODM4MDUiDGCLH%2B6mZe%2FSP4RKISrcAy7CTnTAj8ZLuld793aOnE6IWE4b5qghN6U6ZxJHNZkasBsZabXua%2BEkZJcL4pWUqpG8uDmLAObejrBunsE2Gblt1VQAKOKoFJUCaLvITzVxR3WLHXvg%2Fq0UI%2FEeRiFTb9cOZHHexaZjI3Nf8%2BboKEvwhzNCDuwgzklD2fNCDWWxXZJ1ewIZESPUtzPW2CaxWKjyNFgMqKmmBvxWIwJyXNIaNN0U307VNROOPik3V%2FSSrVDmtQi%2BwF6OETFGlFwyXynmUeXGLPXtSbSyzcQ93babdHoY%2BjrGLoBuwN5%2BrQ%2Bf3wiXzZ87wSQZdVJsyF1vxvfySvV41Wub0D9lKzDAuRfTFcdmtx%2BCTOv4DAbQ%2B%2Fn%2B69PP2tr964agM9Ny%2BS0NKLV2MP4BDhvGeTWkzTeLMsqllneNhIlZFUClvlwJadK6Kgh7IS592WXQ33sbSTi4P0bLimh8YyJyLJ0d1jCPdBTsgKJcnilHMsJS%2FufI7mlBdd7NzHmOl7CvnH2C0uvBJ2YvKkTUnDIjcxp5JluVCUlRVLWdUKd9FVmfjBLIRurqnCs2M3wvV86EPrXJqvinqHT2c33udYO%2BVkMwG8dAXFgqfj8RcSZh033fJgNMJ0qeuV%2FuNzHkZc%2FG32TqMPLmqcgGOqUB5TfPRjMvw2qm9ejiW0Afww8Nnim1HizuFTf2ja2wezRvUm08U1lAL1oqkEVqMWpPh5lmVObkNpYiOkzA6N0VzfrocXcCalfEFelbjQ6EAj%2BLoPROlCAtieBTY9eg98W8FEh65Ct5ln%2Fe1syDec9dJgtzuZMouJ1H%2FZyvXUs7XpdBJIHt%2FNNOFaaQkJRj5zGco0%2BrucM4kYjF7c7DAjqYGPJHFZS%2F&X-Amz-Signature=502c81d3404d6e3acde5ab3e4c2f4762f5dac462e5ec17a02f99d64948f6ac96&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

