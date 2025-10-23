---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665LCDEETV%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T170057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJD%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBv6w6n%2BR%2B8RImPW6K6KCtf9Mg%2BnzNOTt%2Fvw6r2gJe4%2FAiAPX32FADjYBx2Q76QGfpe43zlpvPqMAMC3gr%2FC3akqjyr%2FAwhJEAAaDDYzNzQyMzE4MzgwNSIM7kHYbxCqpDxXThU2KtwDHv6ueFlc8m57AdAtA%2FNoWj%2FWOpy%2FRLPTkr%2BwIZEonqHXOhAlWr4Yq4%2FeN5LcKermvAWYhMibV4%2BtzY0nV5%2FlSuM5OyfUxmD29mIPO5p2Jri6wSRK9HyN7kpzyQZConkkk8BhqFL6DtLkFA3%2BRvOEBx%2Bng8yMhtDLBN%2F0VM2fiJJUiIKJ7ewwWtm4agWd81RDjjTCw3XyYNPN0G4q2xXzK%2B87KECd7dauX9O5Mf2FgPbk0eYGzXN61EmSZLmDCrZDHNzJrOH%2F0yqOblpYzGwRjEpzDkUlRXt3u5W2h6NW%2BUn00jjLmsxOXpKlC%2BR0hGzVlH7FswJGdlQePLM6MUtwtvMXh6pi0U%2FetIMVo0MhtrJaUb9SoEs2mUTir39a7VogcxTm%2FoigNmfoebH1dQ4ddCEzwY7QNoeOSjlz1v3TTmO0Gc7CTnCbCls87wQtVDmOhF6GqJ27ifj40w0JeevW0eUdGYGzIlhRWQcf%2Ff8k5HcZEjpCjd5TgewlJsBGUM6jGzIn9PZ8FZiwD51qtrGEx1Al5oedL%2BN5z6xiM4jbgD3Ap9VdiEwIlnGv9AJFUYHq8bbNrtKKrAoOFOagaUB69iaeuOltzgsYiSkltAtQlCXS%2Bj0w7hLCGn4A%2Bt0w4pzpxwY6pgHqT95uxPdlKvQV%2BX8xYea7g%2BLIkwViSTyyNeLuyrYx3LBbg9iut1VTGnM6p4HIsupuVq1pvGQmQUJoSbG9a%2F%2FVgi6IKrcaT2SsmT941BSt8FdqgZpS6K1F0w24qFhr7xJQ12yXm%2FV1MxafBZhARIunFdtBV5%2BOP036Sbts5Y5xmCrggBKFvHtLPPZscVcy4O98gZTJXYVjil%2B36iAveYZieNmha%2FFH&X-Amz-Signature=457791420d36a421253edc4d80a1c000549f4d9e6b0c4aedb7ea806e1f7f7ab6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

