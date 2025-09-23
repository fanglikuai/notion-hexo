---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TSSW26K3%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T000048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCGC7wA5t%2FRSZNaz4ogID41E5%2BkSEAY2AiEv5Selm6ALAIgN6AMYEXB0i22L8EOViZv3cGTi%2F2EX05s0tHb%2BsV7YO8q%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDAcTSGl6ouc5ysdo7yrcA1yELaW5%2BsG4TM4yYTs9hMS0pCvpfVdgJVlOQU%2BfHstrCgZxiwGC3nINGdXhRYCTNxcsYvWnjMlH2Res6ZlfaGuw4g%2Bwye%2FDVOIdzncZ5feIN3p7cmcbvX1vor2fB5mV%2BmTDBah4tQ2uBBexns%2BXfe0DEC5UdQIOIfoZHQozv9rsKi6cKOg5efgdb2I0k4KQsgzyVdP%2BP5C%2BTY9IGTzi7NQFrUIhtVLE1V874HmtUnc7VjcXiyVQdr6TG0d249mdmVQpSW9ynBtMhFv2TCebW8XaQMs98Mf3ZU2JUpkel84CWDvM4D3xvq0rZgP%2F1nSsiv1SmCytmzC%2Fpaq4N%2BXKt4S8mdI8ljc%2FTGi4oNX7Hyrx3SkMdj3S1i2EHoJ3YGTGoWjDI%2FvLu815G6gSTWlRwzUWFzxQ92WNOXaFkTbo9mTg%2Fxpq2PIJjkLT4csgM94L34QeFSegDdXfD%2BSpNOD6Dk1uk5JBvyEQ4YUF%2F9zkuUEsLP2uycZwUj4%2B5qRbHqBUkAKUcNiKLF%2FT0gX292PUxq0E9%2FLZ7DPTQD%2BTy8U3AaZdT%2FexFXyv59aBmM%2BlukN3Ct4%2BRtDMGF%2F9vL393W9bhbPwKWU7geD%2BW8YE2Bu90hGr5XG5kVm7LYGuz2RFMN6ux8YGOqUBkosPBF%2FeFO9x92PILw5hsAWqTTqJwtfeRVPhboPoO5ff%2F4P5eWQytHAmO9WmnX1DkoApHJMGFis56WU1TH5ZrrZtN7t%2FgIxGZKPWvRd1a8RRk8462vpqYKAK7xjqA0sx6sVePVpR7XcKte%2FKVxjdPhHAnJQ3a%2BxIgJT%2BiirbQS1P8SuO1smxg8nO0uSTkxnIBg0ZzuX8qveMSpcrtCvHYSBQlqSW&X-Amz-Signature=8f77cea82de8b2cb1af63b842ab22f434362167b96fbf63ca0a88dc7921eedbc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

