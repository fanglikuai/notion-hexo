---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466U3D2N4EQ%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD9GZ3a5WVA2vwfCn6BdJWVlIppIynTgD7%2FaDZ9%2FZV5swIgFQ9VBhQU3YrjzUZHFkAa8462xieYbKOcErmb8a6f03sq%2FwMIehAAGgw2Mzc0MjMxODM4MDUiDBKIlD6VTU4ZDPYh2SrcA65WysDtEZzZMl8mNlUBL4lrhHxbLkhEetJUcM1ij70%2FfeqxoHDS2nrXVpGwGBtUOG9m60nWxfZsdP2R9LNHg%2BT3pYY3ZDNKqILvsCfWyi0u3zvW0QFtRmebK%2BVG%2BYH8EUz6A5xyKHJ20tr7Mex1lnVlQW0JBWbWtQDLzcstg5MuSdm81VZNf%2BtSxAYJ2AjUuSwASCtF4gq%2BFTetCtgZjIl7zoHRDnuXVToY%2B51GzlHAz5niXIsGKxDPsw0AzygohrF%2Fk9do7seStczEsb1W%2FZVPLYVTDyS1UQW5ffM9Vyow5szsOneH8yS3U0QP4YQMn9iFWQiLuXk85SNlDuPCupqiIfVSx%2Bdn1DbyNc7jLcNIT2ju%2FVr7%2Bwpe%2Bp58KAN0Byc0zjuDMvTTHPGGjQx0Aj6YqHvwkrc1IVmkr1qQ6z9Yqh%2F3K2nA6BBxklZ%2FdEOTgYC6zbFimBcc0qrlzN0Pok%2B8Kg9a7Y9jBb0fRZ5ukhLIf7tJT2Oju83hUTPwpnLuJ5SZkFr7QaH%2Fv1c49Pt801qj4oRUfOnfKzLodbVX7PO0ac%2Bzc7i1gqKETtJHpUD6%2BcbotHqKcbaHpx%2B9T%2FepzSLC7d0o3eizrX3FURNuIOqOrkdEIlLW9pQX4jvEMP%2F088cGOqUBKPV28kSpSvmV3SyOtZhDEnf9zpbYTvRTENkoLCjUsx5wNewxZpYKXqyk%2FE6hYc%2Bogw1VB97Ze8MfzS%2FLPu069qrRZEB2ePzj5KKKyHlaNCABOMfA5hz54EvIEEoUnD2UJgCVAQIyCa3vIt%2BkjfEWou8AJ2B%2FglTw%2F1zWpJcPHpoIZihGHa5vhRjQDPH%2FSJIA4tHlD%2FQB0gacnW62NocbGg3V1Eqa&X-Amz-Signature=dcded427b856b3133e25098724fcb25b0fecad7ff1fda2156e98187b12a39fcf&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

