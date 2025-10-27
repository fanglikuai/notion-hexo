---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466URP2V7W4%2F20251027%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251027T010047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIC28MriqqGkpa6fIoHqPFjbylvHSqFNVx9%2BBEXjXonGYAiATE7tK0qhzdi0jePP1IFovxvIC3aKLVRkZScazz%2BmJfCqIBAiZ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMdsEP7a%2FutS64TAF3KtwDvdBgycfqwaexCLdhJDLhnJmlfKEkWJquMnals43dhukhtZDbjkmNvi4dGjFkzW4uRzPvF7SoEjZy%2F%2Bin%2FaN0glnaZmW5lBU2j1GLMLmfcbLkH1CWu5z%2F65HdkiMXUCZtqJk3b2rjq%2Ftw%2FmrrZX0Ni3jwYIpjRr%2B4x4uPY45WaFcvFhUexRYYZA6heQN1OBN6T8G35oL%2FkCg7lhGgCjhnq84Ln1phhNZyRjiycx3kqeOJevpS4ZLUxirQpIBsPnf9MTBBZC3oK23%2BpvcF1ov43eSEZI0LFk0fWU%2B5o8BHRmkMtcaLMBTM48EzCpqDr1ywVas0fpN099V61dtXMx4ZVh6ZlRq2e7ckD7iWvtS%2FdEADcsofHjAOXNMm8t3j2Y%2FoizKj1fNWqV2c7yNOMlJasGs3SeuKQpPZk9PUNOjllakckNkLF0imIYizHGag8qfw86yz4FpqO6dAgc2%2FfC7D1IwPIcrhWSgLTn%2FdIlNU0mhNUChtvA%2BcxvjfOOmMerUsQAGsFmHuzb9uduaMDs9h0%2BZN5EwTjj%2FiDuO1NU3EfKDre1TOpzPYtSydz8O%2B3u3CcJ3maXNZFMhl%2FXPBItsozbhhTa%2Bc0cARrTqymtK8Uwk5GK99i9IxHXFeqiUws%2Ff6xwY6pgFawQYAksOQEdIXXgkGmo%2B36Y52pR9qs7u90BZqMK6KK58l%2B9OAsZ2un1hDfNn6vJIFml8emTZOJrTvRaZGFmvOWgllakurYyIL1XfwH2EGmcvT2KZ0%2F5Vt8KmYlVDQbLb6f%2B%2BJ1cy89AY%2FV5g6uwlWXXPauvdOMq4v%2F1FG4DXJXIJEJ2mnv5klswayltBdq0XQdSI5tPGM7VufvFgfw8YvycgBoq73&X-Amz-Signature=593f9f3060703508c150ac127fc7a8d50dbf2adbdf87d17cb80c891166cf251c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

