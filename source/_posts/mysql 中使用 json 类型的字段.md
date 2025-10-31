---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TMKHPWAU%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T190048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFMaCXVzLXdlc3QtMiJHMEUCIHb4%2FAetjurL2CTUgK%2BUnRBIKzSELMkpJYgbD58vV%2FuWAiEAlroMO6hBA8v99zzXBwKhCgaVqJMcKvluFK%2BPKPeK5DQq%2FwMIHBAAGgw2Mzc0MjMxODM4MDUiDH7fmrK3hDeEM4rrUyrcA%2BfS3RFKwn0X8oxDjQ%2BNi%2BuAsYJFhI0AHHwvmpeyTGzOJG0lafy45u2Xj5jPQRg0Qds6R589Qc61RvskYXa7xy122jFta4bDQGTgkivFWfV2tAGE9WfGbpPvgUJF9EA6457IHQJUaQgONuhUFvBOwnwHwQBZGq3eakqnc7EK%2Fm4oYKAPDOuBMHtCKYvZTxYNgfpLszIkIiVFpRCadN0pD%2F9%2FQBVFfW2bQcLYvwWckYZUmS%2Be1a62PqMOE2gv5D13tIjAb6D3hhv7npiPoHNRs4uTSGgmhtYunylJ54NZxr7DBOjXFy8tlWyOzJ5a%2FTI3RVLIWkoT5v42LazcZbseIjprHfesB0WuE4%2BzS03NATxpzLiKQy%2F6T8GD%2Bbhbz3SWtHs5d1DG750n8%2B0V9p3e2EZmbPmDLxIG0GkX0qIZk9gc2zQ3HkPKWMLlvb5H3lZkbeYqXmQAqyD2MvMzkthbdIR8gDn0s%2Bj0B5E5Tt5JcpCVNZ4hHU0w%2F%2B3RyXTDTj%2F7UcBEQnQ%2FHevD9zhi%2FaNBS7zKUUzg0TE9S6Q%2FleTjmdLz01BR8gJrRh0RQi%2BJRSWbZ0R5kfwQVzo6KVqgrN2ZAfETo3lSAM%2BWvqLH2fWrLIw4pt6zrze18M8Rkq6jMOX%2Fk8gGOqUBuP0L9oJAlWBHBQh2UORewMstMBoW5ut4qYH97RJvrMBVjnijBdYrwh1HAQkMBMXOIe6YE0wKg8tm8zbc9ZvQjePyFpKIsm6D1M0Vhfd24rKlf2Gf09YccUSMIC5%2BUY6mAB5HROCtz1CTUmaYzE0xBY%2BvX6%2BJT%2Fc5mj3AGpWkeNpnBxwHgYNBVhpwlBTV%2FW3wyzLb0HR6gfVpHv3IrWFlIo%2FCak8I&X-Amz-Signature=f7219a7d359267c51e30ea029a4984c0c0437ba29f9dd7b41dba4f8f4abcce31&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

