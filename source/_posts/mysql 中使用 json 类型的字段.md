---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X3PS7BLD%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T120136Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC%2FBVmm2Z9YY%2BP9RA%2FKEIWNF3TWdsfikxtOrpfr4LRUTwIhAKQGlO1kHQVJb3Xl%2Bswo3baXXV7TOmbGjshXAq9BRqbQKv8DCHUQABoMNjM3NDIzMTgzODA1IgxTkhZi9U9m4ZbSCbQq3APrvNU8j7AH0bbOY3futwdi4JO7SKlthQeUvCDi7jk7h0SfbMABv9evXi1%2FMIsgTjeEh7PEWacy2K1VceuaK1GaOMpKVgWOtOBL1UjJCpc4Y5nKe%2BbPm0kROyrY%2BwVVCnjtpXeTsLDoo8ZX1caRK%2FwO7BIPkpEGRRu2zQtf9ymkqPKUbIVoa1kshl85bWjTvhVFb%2FpYnqulx%2FIlFNdXD5FYQi4%2BPbrb43gfG5madf3oi0xTKTDEdugsminhNzEBP1GV2p5FcclT%2FLO1BefhTx4wQi%2FLdzFVj42UpininNwZtRW92xASJVF8h6Ab8PgP%2BZIOaZzrLrYyVwabj1gw%2BxNx%2BZwn9xoPBYwKdALOsH5k%2FKfhrIJmiTPjMqjwTidxO0XiF%2FXnN1mUrne0jyms%2BnD9v5jzRz9K0sSUxKBpvtqFp%2F7XpJ29P7IMa9TuKER0mv7mYO9P0at3D%2Boh4Q3ggah8nMnnpww7n8iyKA%2Bfent2Dc%2BpIW2mlVUeZPLrPxtpXSmZjmFKVuv2fUVa4ST7v32JsPvIB9w7maMaYocKKcouZPoUk2FL79G1eeoq%2FpL%2Bdakh7eOJYVkNaF1RL%2BoEiEja0nfByjrJGgm1wetQFYQFFhe%2BazxCz2T%2BNg7uiDDnmL7HBjqkAYj%2FPSusprZ4UGQsaeh0NuZDmIDEKjtBg7prcJWWNN1iZYeIbbx%2BAuuqYWZRANAr0LgsoWjwodfVOSE45jJGsVsbCPZWDnudz7r1jdacaZwZy%2FA6w%2FNHRj1i9hIqpgZr2g%2FUVzyVsogmBNzICoL%2FX8tv9Zzyd%2B9UaZw64N2eRCVJZdpgOEntOEbIiKW7SCAHbMjaR2HmIExMwNbAoEWCcC5IvFXU&X-Amz-Signature=e6b19da2bf6b18d0a1be416b66ad47bf41cfcbd7119fee736f2d32a2793f22fd&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
updated: '2025-09-16 13:55:00'
index_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
banner_img: /images/fedfca57fabadaf76b871d791f9f19f0.jpg
---

5.7 之后支持了 json 格式


但是在实际应用中好像不怎样


# 配置&使用流程

> springboot+mybatisplus+mysql5.7

## 代码配置


java：


![imagescce2478e5401f24de6234fcc9a70b5b4.png](/images/476a1133e7aaa3e257f0f6fe9cb407b6.png)


mysql 中的表：


![imagese0bbc4d10d8ec7819433a5e83f307a52.png](/images/e2532123fe03eee4705d5db2c2ecc85d.png)


## 配置类型转换插件


```java
package org.example.studyboot.demos.web;

import com.alibaba.fastjson2.JSONObject;
import org.apache.ibatis.type.BaseTypeHandler;
import org.apache.ibatis.type.JdbcType;
import org.apache.ibatis.type.MappedJdbcTypes;
import org.apache.ibatis.type.MappedTypes;

import java.sql.CallableStatement;
import java.sql.PreparedStatement;
import java.sql.ResultSet;
import java.sql.SQLException;

@MappedTypes(JSONObject.class)
@MappedJdbcTypes(JdbcType.VARCHAR)
public class JsonHandler extends BaseTypeHandler<JSONObject> {

    /**
     * 设置非空参数
     *
     * @param ps
     * @param i
     * @param parameter
     * @param jdbcType
     * @throws SQLException
     */
    @Override
    public void setNonNullParameter(PreparedStatement ps, int i, JSONObject parameter, JdbcType jdbcType) throws SQLException {
        ps.setString(i, String.valueOf(parameter.toJSONString()));
    }

    /**
     * 根据列名，获取可以为空的结果
     *
     * @param rs
     * @param columnName
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, String columnName) throws SQLException {
        String sqlJson = rs.getString(columnName);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * 根据列索引，获取可以为空的结果
     *
     * @param rs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(ResultSet rs, int columnIndex) throws SQLException {
        String sqlJson = rs.getString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }

    /**
     * @param cs
     * @param columnIndex
     * @return
     * @throws SQLException
     */
    @Override
    public JSONObject getNullableResult(CallableStatement cs, int columnIndex) throws SQLException {
        String sqlJson = cs.getNString(columnIndex);
        if (null != sqlJson) {
            return JSONObject.parseObject(sqlJson);
        }
        return null;
    }
}
```


在yaml 中配置：


![images944ad29a7fcf96a0c51a577d6bc43317.png](/images/4d25cc1863ee3e3fa6ae7e6d4c2a6cf7.png)


xml中配置：


![imagesd6de49b9a7b17849e0d393569b93bca5.png](/images/1067c14ea63fdd81764edc7b0b6e9828.png)


# 对比MongoDb


假设有以下数据


```json
{
  "name": "John",
  "age": 25,
  "address": {
    "street": "123 Main St",
    "city": "New York"
  }
}
```


使用嵌套查询即可


```bash
db.persons.find({"address.city": "New York"})
```


可以看到，直接被秒杀了

