---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666STS3PTY%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T190051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAIaCXVzLXdlc3QtMiJGMEQCICoSp7U9vOS4zu%2FK46mrF2gERzZuEIhzAIbe4F%2FlINMKAiB4a8JKpes7AyENF3AYmC1td5aJjZiA4BdDBBbrXzLsOiqIBAir%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMzDV%2FruiSf1sq1CNTKtwDyXoMrT4xz0yNthHsZ1wZDfUPiPCmDAgCKGYKBmi1HcB1TtHBaAOTnCmW%2FF9zlFAneMVFz2C7TKz0%2BAH9wp%2F8QPGtXyNtNgYG41q3KyWTBB3d68ulGS43LRE3Iqp9Ks02YRTNzIfvNXMSkG8zGq8%2FDjxOXg3yMdrvdB89XD5hWRt4isws9Fv47BxjzXXGsAclANcjZcMWf7fpVlIGsKGG5PLJRcU1Jyda%2FMu3HJInz8TTiLRuQQ%2BxGy%2FixtDb%2FmBK6wXWbTarw4WDRzSxas7SE8v1DUGvB5hB7dU7cqRoP%2B0H0XnW0%2BddaQLtu3nB4ORTQR7wQRgJcW4OuVg5ce9evOWI3FknwfYf1%2Bt3hEGHEypt2XUBLRW%2BYH9ChCJstj6%2Fc2JusBVtVapI7BQjPtIkRzCLeFWFNbgw5iR%2FXcaHlz3FLj9nd9Ny2YJPhpG8hhokuasMLMJ4vDExoUF%2FcolE%2BdkcN1pA4BmTBrAFlpi611hWOW8tkz9mFj9SqdNpnwRAfPkbGHicK38mdNk7b5fCjM0Ro55DWtYNnOokzmoDINYkKny7%2FicrYarM8kLLPN%2FQMDcfhr2TG4K5sg6nD2TMl1te%2Bfw52%2BG2HtaVDfHYTlrqrQl%2FaXm%2Bl%2F%2B%2Fg5Ewuf3JxwY6pgGOfkz4MD6BNl5%2FBuedgOdDf0D9YmwDB3T7wNM6%2BDFMnIcFWK1i7M1FkLHFkol%2B1j4T54f9SU8Yr7gvbVT%2BgNgY243HaP7WkrjezjiyQrh2VSQmsBoOfeIdnMzmQYGyy4xMmvfdhTJ2hOwtzrVqnL7RS2yft%2BK5tjUcOs5KbGrwUUEyAWSu40op9JLfY31fdjhkGrhvsk8xVbQollLEe2HlbMldb8uE&X-Amz-Signature=90a67b87ab9aa221f4415270d3dd5dbf24f8ac591c3b0c5f09093c8292cd46b4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

