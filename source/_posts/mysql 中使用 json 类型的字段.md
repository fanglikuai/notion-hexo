---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466X7R3A6QW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T170105Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEAaCXVzLXdlc3QtMiJHMEUCIQDHjlkqMi7xefwZ6yvzuRh2oBvnpyP%2Bdj3YVbsQp7QFVAIgXW2Vd93eZdx%2B%2B5SjlJOpapndpAzc%2FGEztxIszFb6ssUqiAQI2f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGkmGqOm7aVt%2BQFASSrcA2odl6D%2FMxxfYLDwZBTvHs%2F7PZRnfeoOKhDCgT8656vygDSP8qII2%2F%2FOMIqxO8Kfcn3gvxm6T5hfyziEpY12ZRGKeScm6pQ6ttsndzOIWI5TKVGFjcnP11HUFFe%2BVlLMjRGwdCamh%2FvOS0gpe3lB87HHYq4xrFazAGFZJbwCgP8uEMhjESYsFuI%2BYU852MPs2tAQqoONxDG4Yod41X%2FkIhfzxmVeHOhQFU7n7y6KVHBf%2FM2Vzw4aFv7O6P6NUYoFZ57spEeUlo81nips42zGogfHU8gPquf3mQALeXaUkWnZwU4kVkgM7d3MfPqU9tgqPlU%2Bwao2x4ajmSDXtfcdNv9dRoh2IJSbvnKZSjkjphCspX2mjl%2BgTt07Wug15rIwTwRdThe20iDggIpt0%2Bu3eT%2FzLac1aNEnRbHBWvWHbFiHq6ioO6x3%2F87UpAd5uiFjHb%2FWcczpGIJ8uNtMTcVKyw8Keb%2Fs3nHX1BagAuA8L8sViL3ulp10BCgTDVWbidAiWnMwWr%2BziemeHeqaUrOlgaWcAUaU2ECzg9qdJqhl3dXdvivX7PuWTn%2FgXvANwA8I3b5QeY1pQQinK1GGOPflt6j%2BqLSraHF5MGCzMWkNHWuresBjQHL5%2BK%2B9rLFxMJfAn8cGOqUBJhaknO0I610B9jMv7IeUzN1lntUB2qHD39OXdvwlQBb1EdmvMikUd0CTUgfLAdvxPdcXda64jDyne%2FBArAiC0tDFAn5nx3fmSRvCDGYhWQnNcZyFyOMhBbJaNFUe67DAhJVBlQMtTwGLqaRiLeT5mrXaTo0vtg4CXfQeMGKxzxrzAxptF2npj9z%2FnhJiD3T30%2BYup4wmGk4dX%2Fc0csQoQqmYFopN&X-Amz-Signature=4bd4c722d423425e6fcdd5a16165f781e4416013aa22766d7ed8de718d97f88b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

