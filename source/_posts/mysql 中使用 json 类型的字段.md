---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664KSBXMOL%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEA0aCXVzLXdlc3QtMiJHMEUCIQDIYQXtSg%2Bi7I0%2FDdUjzZ5UsY%2BkX3%2Fdxnpv2HSoOP2sggIgM83LpJluZEnVVZO2a2uHVvrXtxF9hlxQgqVHH1%2FWeQwqiAQI1v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOxqhFZLhmOUHmilircA4qbCLVx5Ne5RNDCijyKtI6ybM%2FcIwm0kWb874z7G8ByRdVA75R2lojcqQ5gB8fyp3%2B1jeaDh770em2YOHUExXkAG%2BcV8LMsDiWvh2qHjXqfGYhYeI1RScrOBGofdAn8TAy1OsmbJCC3P19Nj9PPPRd29c%2BLmWmKFDwb6HcQmgDFHjkr8jMwj74Vp9Mz8vtgDOTLd8q%2BFiCIJ892W4LP8j%2FVOzRB%2B%2FIHUJHjQow68fkUN9EX44zTiSYJgTas5i8IhXS%2F5pEy8%2BTXM4PpFgOFqBntg2JBYJL3z6LprmZS8WMMaMY2VhwmfNW6BMbsCLAp8Fh7VYSfzsKUVLdyXSporelGyC4Nc5kdKPd9SOCXDzAP0TlNjL8unmjNKAw8aIQHsZntFmlnUxNO37u5YHQGbLb1hqKWPeuUFU4MUsHahyXgq9WArQVIA%2Bp3Ffcdej%2BcCrAgZMtaxlZY8x0BAQIB1J1Sud3P5fA1uGfKPoM2%2FQZmKng59EaPhNC8lNkpcyUVwVHtP%2Bz%2BKJZkUtoJui4c%2FJyBST3iAAhHbE%2Fd7r0s4P5%2FmDGxImJfeVWyJiDlczS9gqYxF8lh23SLdUi4pX6uohbfz7ApbvZD768VX3i%2B3k6PNRHaFOPQzSKQvnL2MPaV9cgGOqUBsgCVYjKDH2i1FhiMmdKqQXyftKo%2BICQhcc3SffeXzdVAL5WOzusye8SPHEeO%2BUxV4xAByfiRmx%2Bbe8xFCAekakEdJS5jJeMJWRKXOlhjTfFXtUUm2yRDxNX3VrRYZAEyGDzE1ypYG1IzSPUK3MiSbAhvJ349Tabsl8OlVd1PDj0sosmy0dgyosquJHF3I2lSYTUKLpvEZWQaB%2FKgGY2qJxTBa%2BBR&X-Amz-Signature=016304761ee96e775bc07013e6f1ca7e5290fe943d7cd30df1b6ed29f1bae639&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

