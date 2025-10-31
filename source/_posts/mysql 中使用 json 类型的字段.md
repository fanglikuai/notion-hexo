---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLFYAIAI%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T170047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFAaCXVzLXdlc3QtMiJIMEYCIQD9MEye1%2BjInCnqzWpInA1z14rNneoQMRN5LrH3rWfyFgIhAIN2qFFQLLZ%2F3QYEkWR84O3u9D9MVKyzIfCkLj%2B7G8jJKv8DCBkQABoMNjM3NDIzMTgzODA1IgyG24G%2B%2FLy%2BKht4h70q3ANu48FpbJKTizUhNp9MeE1RE71ZcXwRIJWp0l%2FcNpIZousbbT%2FN9gTkIeBLN8pQKPCWzf0ZEsSd7vtOQkKz3ErGzfgAlmUBK6cMOfFSodiotW9a4poLHk5Jk5XmjD30iD93VB6S3OOJxI076ALjhFmNSUmBLXIoxWRK6PxtkaRCnPcYFz0VhYZUFZ4pYCNf%2BB%2Fsb4eaiXWjMhMT8b4MkBvQDDh9lofGZGlviEHDzZLKvfTeptvKmqpMxHJ%2BelseFLD%2BFMg0%2FqvQRUMuXEUvqsnIPw4994rMvLU6v6DKW7YSZbtATrGmJpNaJCf0DbrGYjmK%2BvlJnNN%2FVeUODGCUHn%2BnGRxXCvioIOi%2FOJYI22IPj6JNW8k4D76PuFAlc6iWzmvMgxjMC%2BZyT97Q3hR0kyCvBgr%2FJKRPwjJUKO6yHK%2B45mSUTYR0fsabgy43NZs8EEv4X2hY5il5%2Bd3baVpTGTnBt2fhRojwkSe0b9kMZcXK5ao2n5MlwSJWfAaV92ing3%2Bg5xIs0liUnppy1NyJATxoMgnUPyvYJr2QVL1UKd3%2FcbLs2537PxFVRQH0HG4gPx9cDekONLAxU3GrciqH7yVuAjm%2FvYqf7kW44nfsbPkY0liwOAXvpwTSabK%2F7zD%2BuZPIBjqkAZky6Z0q18w9%2FlvAAJyBnVs%2FrwkZt08td8d55oSAzgATR48HYPK7Hzk%2Fl1Cs5Y9VdUCyKvhCbLQBYD8N6JQYvAwhT01LvhLSZYBB1knH2uoEwiVN6ZWk%2BU9L0ph0hutvm7H9tQZVLx4eUizWMM2zEfhPraW8lqjZVxJEcJ5bNdhLfymgvm7X0NZznzzeihgh8o0Ze893vg0yFX%2BWPCxcNBefmE%2BZ&X-Amz-Signature=d742ca47e568817bbb002af9e455e7742d1aecd2a271f20d5254b48a4eb86872&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

