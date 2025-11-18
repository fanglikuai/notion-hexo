---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YBDADZVV%2F20251118%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251118T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIE2PVEvxDPlpMMLc47NKjlYVpHMaiZeAexLsrht3rykJAiBB%2FtrDxnO6LhSFE97UClTSHEqgAdZj3MP%2BpFL5L6T6qCqIBAjC%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMEE1nBQE7hx8mfPyqKtwDHxPCnEGUt1XbH%2FVxwiG7CWQepxDYqWw7VZZzvvEDkv80xL7m2U5PJRzV%2F09DrsPjZidJUWLRo00SAuRv8HFIgfZu%2FZtGeIofIwMtuK5AtA5ugVn%2BvQ7BK60O8VS%2FN4JYN8jfQTQIojuI0CazHlivdhGj2umBcuKhO%2FwkU0kIqSPN7CnanVU8qp6SXT0UUEjX4Ckv7EeScwktRxu4AWFKf6xWvvHXgjsv7A1Rz9mrEQ%2BqG0GMTY7fP783jNq45aafqy%2FrXIQRtAVyqVHLhIbwD3L5WsBJM9xWtOYqMj%2Foa%2BFK0kL4Y%2BESc6XI08E9QF5bMWZpwyOChayuQz3IuxUgUYUJMJCgKYWEEItUal7LJfc6a95U%2BGtuTuW1ApUl4F3pclDeELxHskLqw%2BafcSUYmp10omzRCcPkGBPLZt26noTCuxKnghgF1u%2FrDNK5p%2BcH1lczaTcTUtTprurnOlYr2%2BUafhD%2BHnS5fFhidjdt%2FCX%2BA%2FxSw7X5Hs0wlPsp%2Fgd14l%2B9VeRR%2Ba0BLCHMTa6ug4u%2BOF2XiU9Gigvxr%2FRvAck7srASoSmrb7u1hDEb2nOlT250iP4V1qXZXedLPDk5ECfpQeQeHrwK2CUIUqcVOuImm24WuDwfU1NUNhYw0eXwyAY6pgGEoaV8aWs0NQzp8uwDYZWpXuD%2Ba%2BzRKoW8LoCLcjUeIoNLwrctOq%2B1ZEoqePnEn8gnENWaFEsdMQ1AE1OUeXTJxI6G%2FEqgRU9QavpXDRMM%2FPr%2Bi7CJPs1W6hxuzzj37r7acWU8eZWKZ8xHJcQaIcuV2ozg%2Fz54eei4CJBM9KU2BVmjeHzGmLl%2FSHx4%2Ff5yTgZ5iSVV95el%2BOCQgR7C6eQluXRyV%2Fbo&X-Amz-Signature=a5a9f2a7becf3d8701b79787852ddf0c6a6797e8de42feed0c6bcf8e2aa07234&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

