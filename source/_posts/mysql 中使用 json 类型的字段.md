---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WFQGH3Y6%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T120047Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJIMEYCIQCkhnhxATd99Kz256e9FfQh8Djc3E6fUi4NIKpE%2B5JgFQIhAMef5be1jrKZb%2BMK%2FZDWh%2FxhX8F3tvNhjsWZRCaVemzCKv8DCAwQABoMNjM3NDIzMTgzODA1IgwiaCB9uDLmtVmhoyYq3APmob1sB6GEbyqY%2BqlaIKtYP2FpEF0%2FE5Q1xl2G4HjIqTOEi5BiudNYE8VERv5lNY4tuh6IpPctjijCeSm%2BPGeCVmadBOdSirciXhNXFQWPdOezrJSLNAc2IlttiCC06HBO5ajk9ggN4FbLJqD9BjS%2FvKdd12KEcw0wL93s2r6soXz6oJsRJV5rHAGQ%2BKrDRwJOna7DuKwu0ccwVVMtyG2u6rmUGC5sZYBQeeMGAL97A2MdU%2BM%2F0GCK2R77TobduYxkoy7ChNecAbu%2BB6OQBJAO%2FNuC4TQWJjz6K%2FSiElrr8ZNScK2T16pPccRqfY5WmPafwUMWZHEmLASXzJDfGl1BoWt218lk2PeeofJDcQ0ldbkWu2ve5uzjyWz%2B%2FQe%2BNoItVJYY0l6MbOAAzl%2FVHK8cEUEkG%2BuIfRRdJScNyTxKX0ukYoaaK8iY1eYApHabvw9bZXAxqNkPGpgM0quLjZYKx4urY3%2F%2F6436TgxGTGs7PQpMBjYgmos8vOyJ3T%2FhqVZQZPAillqiYjx7lDUyFuH2Hew6vPUyslIAphR%2F7eLz6HQ%2F1ikCjnBN%2Btw%2FjTyoDfM0s41CSE0wSQY8GpFi%2FdJ4gByoZm5o4TwaQtJ2deTcy39qrwg95ko4ePBIgzCqloHJBjqkAQ1tz9fACMvB3SPbqb8oEFk3nxNnM9q32MF1PsyE%2B21TZ9GcM%2Bh7AYxX1DLC7OmQ%2Fwl3LDlrGlaJ%2BNvPQjw29xASiPnh50qn4D%2Fhs3Ihq1Qdt0dH0l%2ByUys6GLJKsA4wodavwgiNrcmAVVi0y8Qc%2FlaaYIp0q5s8hv2i5ZTpCoYmTDknkRd7uRd9%2FYfpOH9M%2B5LZpXEI24pD40QUTuP7iNqXqI3t&X-Amz-Signature=f314e93a9fdb2d7e7e34f872623d020ca56fc0b2f11f7ebe9a265b9af413ef09&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

