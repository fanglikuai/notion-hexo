---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TBMIJFL%2F20250924%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250924T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC3%2BeN4tr%2FcsaRbvkxaEp3OpBj2TAS%2BxhxJBBJEnddFSAIhAJa4iOi4qZM5HhFigZrmMVCLjJKD26OdTIGT2ELkRHMVKv8DCGgQABoMNjM3NDIzMTgzODA1IgwmqoUWrwckq%2FG5Q%2B0q3AO4pnncWly%2Bl6eLE58cHbgANX0vRooLxS44LAoazgZZWLBTIqYrrQPCfcam1YBaJ%2F4uSmpz4zWIAA4ptE2ydtfJYxcO8uA48FIFwFvcVAMhEmjtSVn93VkK6jqU4cXAiu1FT1wpsIX3GIV4rGLVLutUGDN3LsAASKnaA0isU1iCZuxUywRZ1MgtSbF8Uo5BwpKwMMieQq6T5jJ94SPrMLENiR6RS3XDRs%2BlG0y7sgS4uEF5wH3QH89WmuAn9aM0nVIhBDc4ferSuC%2B23Vap996OPX2pFRP6iAR3cBIPW0Osc4WksV6yeeZnJC4010dYMeOtSHRTgDOljldQM%2Bha6AKI7qtT0YokPNiFHKQJDju7iu1OVdOuYdHnKio02amR9nMsAclthoSX3keOHbPM7ClCwet0zfP1cAekdkJVQI%2FnpDSHgCF9zHKI7MnVANYM64w%2B8bXI2V52YlZzEgSjtdiCor636tROREtqilnDqpfmraGWtIaa3ojx85d1rdkk5cI8YbnLo3GBN0fczGEVmTnjWjfoADsrd7%2BMSxEB7%2FlQIH84nbXG78XUrvqbxo%2Fsg9ZtKTXo95bKjFjwYIzw2yq6JE5MoALCTsb7TtFFxYIRlirh4OS1%2F1WzKcAAUjCf59HGBjqkAQ2j4E00pJAqnP4tzp2%2FaoPMxRy85lY%2FanU2%2BetH0GxxiVV4tUzO%2BMnpyOztBfXbwJeY7ZKgZTMUD4d4oZtik2yg%2BYmhsxlfcyd0vyj7BJg2IKnCym3muZegx044QlMBa89bQyZfveRAAlOFFvB8URF0iBScQ7z61ffhNe8mXrKmmPJ0GivQnDKJK3GacGImsOqbRVpzo1Y402AZiqfq5N3kO1Vn&X-Amz-Signature=9a8449e0531c71f842b458bb8af7c67ee248445e14d8f1641c11db0ea7ec8b0b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

