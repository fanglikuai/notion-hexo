---
categories: 源码阅读
tags:
  - mybatis
sticky: ''
description: ''
permalink: ''
title: Mybatis源码分析
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/b489ea72-a1f9-4d27-80fd-60ae27c98c32/52520072_p0.png?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665TVUXJLU%2F20251007%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251007T040043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQCZxPwr%2BCCGYkwvnm8XvcO8scCCdrCBosRvPbIU8Vc0qgIhAL2GpKsqYVZs9r3gMli5NnEhQaNvBneESFo0rFdtcDdkKogECJ3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igyz3CrZZxIEQzScU5Yq3AOmKyzsD%2FePBY%2FU1bBwfeLAF7EP4NSm2fcxxKWri1MaW9oXzavJu%2BfJS7nbZfz7A%2BneLW4828VDxCt%2FECL4mrG%2BInZ3QldE1I0CxkWnYutADJnEria9y4lGIdRUVm9o3cVvZTyutFCzyBWz0MrByWP%2F%2B0YISSVAahVP%2BZR6pXBTIbTei0Tc0LTKkktcL%2F8JtyA3y0DwX46jytyOmtwSAB8P0iMm7CTk8tmQSnzdfMkPYKoDU4XfyIvQhjQZX%2FYOBj5dHH9cif0C5lftWxJsFB0OMjKx%2BtLLD6L8bioMFrGsMUjvrzbKNijEsNxR0HU%2FZLW9iaHZOALoCuKwOQkXZTRJOFp61wxY0zbsDkEffn5uDmXr%2BaeNGFNsqN7U1U64bHVa6zbZ%2BxgaJJe8%2Ff9un19yhWOWuYJIEr%2BDI3FK%2F%2FjldbXit2i27K1947aU%2BbkIPoSeBNW%2B8d%2B%2BHiu5fTmorldf80R2REBjCxO8%2FdJIxGYArw1Ta6md3%2FCNXxoU8SHJQ%2FUFXeYmPkptqyT%2BV0ua2YUYe%2F0%2FoL0Qm2XM%2FnGrCcznouNRNg2QfN1bn5ENLfcukaWr7XedexH4MSCycgF7Y%2BDt7dhotIiDABeMY04EQPlpCpsbdLa8Fhqg8UMAmjCjk5LHBjqkAbfTTm7CxizWSDzGvslo1NUyICHqoz7naNklJG2paOLBLhMiwApZjlbgegFvIcJ%2FVWbHFJX0cv4E443kOF7Jdgxt9wTTedS5RPei4nGcJZcwRClQ8PWh7ZpVvh9IetzW%2BUkq7IT%2FTVNjOM6FI%2F9%2B2r5gQm%2BwZzQXhbwO9od11HtbcHbcNemDYsz1mM9Kd9pTxRHJVWKxeH%2BZRdWmKhTudrQPeqLn&X-Amz-Signature=366cace15ce0c459750b7da94b1c444f2e5101dbec5820236a8735758cb6da20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

