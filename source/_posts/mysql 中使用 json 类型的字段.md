---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YPMNT4CG%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T180046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDkaCXVzLXdlc3QtMiJHMEUCIQDTv3GEQ3VUPwE0S7eKQKqKJJpUbJvqox9nTd1McU7bAgIgWTT0yPqprtHRSN4xX25do2V6s5lwE%2BJog55hgznXM4sqiAQIwv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNK%2BD%2BjFLtDsmn%2F8PircA3%2B0ObGOcma8IM3r3F5XreBC4Bi65QOuRyghgvPmmKivHbt9qh8XGIXZ9p%2BZzfME62rjpvlKYdZw9CO7mEDlszkVXJeRBtSpMc6h4aVavhKGoGIt%2B0KAIGZzk3YERe6l2c1dtmmhO5aLFWapjbBY0v%2BygrWiJmuvH2yFEFpMVs3YDNjWgg0iWvkLYC%2F5jJy26TtALj%2FF84beTt4VZ%2FkI8YRdr82ScOYMJeQvA4j1Qlbplu9mHduFfiHXF%2B08s3Thtq6EysV%2Fo993OleXrf4%2B%2B6Ui3Y0ZcAmabNh5Pbd6Cx46rZ40m7mRlDcA0tUCcpTi6HDIl4At%2B2wFKSXsWN5sXYeaT1Ih9orzmxdCoNg49tuimfWLhQ9pg2SK72xkzRTjYa0cm2pJpPRAu%2BUfj9rrB7ksWgFjpEyWs73Myyh5aaebLVPjYBb3or1OQYURuvguKeCocLINc6o59L7V1bg8f4MLQoiUYYOzDTfnQvXv%2FSo%2BSsrEKuROx3%2BzJvC%2BPjT5pujXcr135gNulBO6yHdDPf8zZ1X0106jZO%2FpIYnxW%2FA1mlP9y76gvqu9W9VMupaezQqVIjtXYFQvihvMGeA2eyNQImFTz9Lkxow4y2s7BTEj8Yvf2auTgm2nkiAIMNDX5cYGOqUB9UHUUUTJx0HWJ5t8%2Bv0tJ%2FlWeViz6Nnlg%2BfIoHOe0hkzLI6RZQh4OkUPVn97PcgOYHKYNUVWKm%2FUQnNbx5ov5JW4mJcya5JwcTvcbJh1tjJ59RI%2BWISJ0qTNQcG2zX0CrTGB%2FaFg31Yc30X04P4ZR0D2V%2BjxhzU1z%2Fq55W4t6UKqMZR%2BC9DSPDek0ZIrNLnr0N8cx13AkWg2ir8q%2FvNJ3JO9fgkC&X-Amz-Signature=db2b6fbe731db90b50b0bb9d35f09fb39ed350281a1d357c78cda0b956daa188&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

