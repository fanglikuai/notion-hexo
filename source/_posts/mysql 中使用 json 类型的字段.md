---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZQO7SB2X%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T210038Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCk7ZYKtsuZALpxrSynEQx4MX2goaKip7u7BQb1jYUusgIhAJlh9JnLvjY8R%2FdEbqUYbdRRbJSKRELuh69YPEQQKYr7Kv8DCDUQABoMNjM3NDIzMTgzODA1IgyyxMaQ2rQbdtxz0z4q3ANDcbzaZxH%2BDL9Z8iAaE4X5uOfpO4jAZhWXlLWN9i5S1YeFTtehYLhwZufy9KoeBWqeLEz6wu6HFncBummYdjAj6iErZ%2B5F%2Bmbhhy%2BLGZDVMq1xh5y1DEx%2FQ0LfWZ1OMhCFao3z2iZu2rDJXx1kyLPHzHLv8dj4bN8Pqi4DM%2BtgHD2EaUTk4cOYeyv1%2Fvri%2BqtCiytOQkMYhA2lfF6NWYZ4Znkkf9PjH%2B%2FVVFRhOSZdcwXDk7VIFZwDYvJX0xsdIuQcdoJFKSDNEEcS7tFFHTc32ycfJGdvhDYCXXghtxAHYjKoFjOfyRLQcvESLHLEWQBUdAtOzOEj78RcfaoXRqEF0HR39G%2FqGgwD1wsVJdnP%2BtCpQ8YEizHeRzZ5Wk%2FpdQgk9%2FsXVC%2B6ZgSqkLxBwkIQ6VEgmJjAI8L93h3tAiNVB%2F0uEKFVn3ltd4pE89n5KsWLnb4i1pVoKpxE5Kfhvywfrlq42CKHNnBJAN1rfBQtP4YkhGJqNGa%2B6RZBxS%2FEnM67nGTExOhu%2FvcXBvpthA7eJgIyIywBlsFqJ40voWHTevugu%2FSPJzSl2logCL7a%2FfagL9kpjQeK3AiN5u3yenXGCycv7maJmEO57Wr3g5paI5K81ODjWNBmGn107TC0ibDHBjqkAQFuUpZMDTLJrDa12s5mfkjKSMHnPvq5ZDIjfPxLo2mzH%2FPM3srXU6NdQF9XHJ87wohUTIzmEUyonnNs7Mk6HNVA65YZaGbYAwcKmbOCWCequxlootFcmmFw1PD%2FsK3y84SxNDMSurjCA3UetWE72bYiZn2%2Fi4nNtRjK7XIj%2Bt%2F8HT9vjRlsk2FX%2Bubww3rPXQJv8r0DJtpCXFiNloPAppbghdK1&X-Amz-Signature=063ed915d25ecbbefb608c4d07cca535760a85745f6eeb5e59cf1dfe90d8a58e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

