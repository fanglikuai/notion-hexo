---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6ZRHFFM%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T000043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDcaCXVzLXdlc3QtMiJIMEYCIQCydzWoUM%2F99mpKrbfbR5LlaiDp1kVAOpODQNgN1sPpYAIhALCYV2BYoRt4IvrUrDPJTKBYh9SToc15pdkoxqz%2BjW81KogECLD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyn4ZQ2rIuRG%2Futt6Aq3AOvYMcbMZquXuF8VNA4weSzaaahywVc48quMQpE7L72bp843bY%2FYU37Sc%2F6ONjApYTgvTv19%2FMlaWswBpSbqaAXWa3AlKkoG7L6S3ZW3Pmn%2F%2FKe3CpnK892tUx9aQV%2FjtPhSqBtutI%2F5%2BnKCI8AO1XeqKUSVTNYXIaHjcXzsS1bZMjB%2FxnMDQTM%2FUMLoobhnv3ZqaLQaAkHvUbDbi25dRGiOX59IvkUtlvJz%2BZszkNc2L14i%2FTceqP3SizeIaTaXTarEHlBWmj8Y1Fd%2BydTkmBVDQcEKd19My99Cjomfo%2F0INB7SIF1iGmisPDQu29yPoyCBz4mbrfC%2FGTE8HiEIj74bUrbijjJ3Ce8J8UsvLG2a0wkzELa7QyHUHA%2FsQzsYIfIBeReoxlq63vkoKyXINyHWfaBV3NYJjuy5JQvbAkxDlkroD4kG81L8bzJPT599x5L16UdlTRej6UMxvP9v0rD0IGuMH18IiOUJe4eB3AC4fCvgpjZZ6jkPJiG4kD%2Bh27QVpwLe6PnpTpNhUAFmZW4Bg%2F2%2Fde1vxBfYrUw%2BXgdLL1CaXJnDJe%2Fg04HS7awlsAeXufi%2BIWC6uRqQfbSgxpBefr00bv1M5r82%2FoJy%2FrUncqIG7tCPL4iK4zBdjCR9qzGBjqkASfZD8RG9Wf8L%2FUvI6%2BQXRVUnEa76siyNlBMBfk6fA1FBYrwDzEScJ%2B5OvrYHlfpSt7oihr47luXX66toYM3ziijZ64eUClZxP7fX83wsZHdfL04WJbW4hTS6Tvu6LxhCDXLxJBeh%2BUMNF6lf7119GiUVSSCm5F1olHu9L4fM0KMKpyhf1MOCiU3j4NNAbwXU306EjRZdp02yXrd1UhcS3oFIze9&X-Amz-Signature=96b0b8033e5ab19c5fe9fd00df0f2f33f478a0b1aba35c01de0598c368abd9b2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

