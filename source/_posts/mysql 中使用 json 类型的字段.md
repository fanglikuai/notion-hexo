---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UALPTGLH%2F20251113%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251113T170123Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCWZVekGJlG2g8PYKsUnMaO7waf0gtQPAM0dOLyfdjFLAIgJgPziuyqv%2Fkyeq8Mxz0cDwM3BV1yKj%2ByvxVylhM59f0q%2FwMIUhAAGgw2Mzc0MjMxODM4MDUiDFq19cXEd96XKMzPvyrcA%2F2%2FSY0Dc%2BzFjnWQsij%2FSvb5vW3oTn9NZuvTGYnh5r3bIYh7VO9qJ6rlvuq1hv5byjN%2FOqef9f3%2FL1ZKELZOD6HsEx03Iu0mlowF5glCt7IbasAyTLHJmcq5q5XK8mFH7GOiFuDKQIVlM2FWa4TfwLBq6BwhgcOjyf0MUxtqiUs4XiE4lMSbUK%2BjPgt4mJPfb3eUN%2BmpeNfY2qROJRln5QcJTHzRrOQC%2BbTvhfI8TBeWWPza%2FtTpSpmbY2ACRzWZOnSr%2B%2F%2Bdh%2FYqBWmzrLYyc1%2BXg6zwq64zGRCSj%2B%2BQghCbCAEPoSLsSkfXqQ6cFp5bDW5lcwDsQ%2BheYN1dm19WTXmxzuQcgdKSqK%2F1Si43A%2BbaGuFyWZ1g0m26F2QbPBSDYSC0drtNLM2QaUmWq1tjcIRraF6TJkxYSD46BASoCwQeGxc7yU6eru2zfxNV5yigBj4N7JbRNfhbJKHiS39XOxkSt6lal5P64N%2BkT7z64uHmc4gxts4exm5TSjVEt972ZM%2FsH0VpYhEAGzN2EZtJvWRdkNCKp9XbZOKkyvjgGLKW9NzCPUhKlgFQpap4Z1JJkW0QkabhINHFFQqYZeIvAwxGhz5UF59gtjgJDoTo0mbJFpLZoOu9xPP8fOV0MNWV2MgGOqUBTtsfN%2BQDy9d3gvCNPfANaubd1FsKWeHAorN%2B9rPRL2FILD0y6LdrcksN4HsDCZuCrsG7%2FO1%2FXsBdyQhklBnjUkqZkMCpxlOuxnA2NiDjPrEkRLytvGsZ1VJitDLG3xfdWFGBzRny6fUh7XIpHVihrQM%2BpBSAWE0mKrK02ONg0DEOaAMoqT%2FMtC%2B%2F8uo4L2fqkFCOMzgiSwRM3i4EDFtbYrjFwWxr&X-Amz-Signature=be0ff62fd04ade25e1ec499f6a067ab4ba33c586a29c487904c937e3e9f73a3e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

