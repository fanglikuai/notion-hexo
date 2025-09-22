---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XFUTZONS%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCEM7TFJEc3xPydEo0u%2FmhhSewUt5ey7VZsXMuoMH6snQIgSNp7D5YMn8llp8ibsa%2Fuy%2FHeEKJOViovGTh8MNzHLqgq%2FwMIIhAAGgw2Mzc0MjMxODM4MDUiDF317Kcy2JSgFf3FNyrcA1GBaxGF7RRsC6oiQDofn08Xbrkac3LbDhEkgbw7R8%2B29buAY6jRGkorTXUIxDDouLt1RQmG77PGrI4raBXV9YTMPyRFp7FQEGNOBOF95hGOstuYmemctaZ0%2Fc7HyXlrDfiS5HuwUm23RrZ%2Bs5la0ecKK2Ha6FMJ3G%2Ba19o%2FqV1w3u9DTlhHbaSJcaZDBxa73QQAC4xlomx%2FkzbaZ0okOScW9cwsuOIO12ONMSvjji%2BtlLkwVXracBFiVK0zsAyg3nr55i5Mx5ENwg9XfnUHjqQi7MWxAe3X1Yk0t4J4LAONr7t0AYq9Gnl5YzKSV7SA4NSIohynl4RYCxudOMUyDWWHOawclGAi6l1u0KC%2F40ZQkM1AhYCxfcF82uPaYlweJigDNbikl%2FeZ7WeKvRbDclG9%2Fe0wrgMDGprHiTeKmLtb7FSEimEA15pIR4XJ1P5IjZHsMEhRCyrtkdhspgxL3PHKLMvZpMh8nGGnEncfCXIPg2C86h%2Fgm3SX0ORHujbjXy4aoPbsv0pFa1gfg6xtH%2FaCNkoHLiZHrIkxgQJVV%2FlidfUqKwCxrLke%2FFhJ%2FR3TJiA8B9Yp888ilVFxYx%2FXmStv8UYbvJnfngw2Kiju3uO8VjhAfQQ4J18ScTNnMMKwwsYGOqUBavTQU61VIyseqgFfBhwUhgAjv%2FS%2Byv%2BFlGSED2SvuP8QSrSGGNWCAVfy%2FENsCmooN4NTf1BLc4IPalWR9loNi%2BWFDMSP4%2F9lQZspP5zmMCbf1z%2FULLHdVdU1PFwDpMy8pNsHthmJtUtAAKZ25wfmNWo7RrUfMigftT0e1NgD8OQmBzxvXWijZiRcSD7RKpxFxbfTEiVHczxrylJqv9huHjhdhOcK&X-Amz-Signature=58ac7be245d0ae5b7d38d968c2898ba48830595ace1987b90b57f9a6ba8baf6d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

