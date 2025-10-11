---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WWBJQPWG%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T060046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIGioOW%2B7SYIOFsbDp33M3hyvgqkffsNUnMrcAhsOiCAHAiEA2Bwh0PnQQIWQCpkcH0%2FQiCNk1fZtNlhc5OBAh0o9cdoqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDXlK8flbNfg9YwM3yrcA7ETZB1q2mTeNZ2vpavlx69Lbt4D2wI%2BApHgU3B%2Ffv8684Kd2H5Birw2PCuQ5TlYkOAWAlnt40FZBHVEVOz%2BCs8AB5jnDVaOaj8dBewoNuKQjB1tWHCpAqd2wx5QL3FjQGrXsC8H35leabYqpVzd3qtEUpA%2FggbXkv%2Bd1bBM77f%2F0HFzfyO0uxgAOnC1vfZzVAXXCPex4l0h1laWWHOnVNjQCkWM7jo40DsVIZfFGE3eImvvP2X7oWPTEw%2FAPQWLoMob0iSWYaoVF5PPvIpKhWvE3iZ4gEK2wIzCg2H8GOAhVpRsKtk12xD7r7vD1S6SQ6N0ZsNQSh1SIvG7yh8Yi8suu2G61KPOeXrx9bnV97WjfBh%2F3gFLnNrnc%2F3egjQ9iwBm22x8PpZloQoczCsDjaPxsiEEYkbGTjsmbgvMRrLzEFyzybenyqEoNa0daQztXsXEjU%2B1VNPPmQCWDXl%2BqQSYHC7nrql00o3oSxTw6Gu4HGlqxKkC7OqiRdR7%2F6%2BFfYLXSLw7JocSxJ3PCXVydZu%2B0a1oWE4W8wFVUc3Rwo6UWhe1c%2BBj9ujvapJK9KrHYrf7bH9yF0dVnII5FjodrvMe%2BwmeP4%2B%2F7RM4MhV9LupIbnaZYOWdddzf9ZS%2FMKPip8cGOqUBxcVuKsp3LVJPXuVUIceYLGGfhK0GO6q9XhnTW%2BwseqvJOliMOh82s1T2ba%2FW5RaEyBFiX95z7ssdF%2F6Mm4bT9TYueMv2P7h%2BIMgIaLIZAvv8X9vM7kzXbRq9wMTdtfBzwAB0mumR%2B8H63DFsWpbO6SNN3VDWcc3uUyUcrujWI%2B5aSxRaOoTYxxHeajkIkjTrjLOwPWQ2k6YKjqXDJXnPfb4tsxMg&X-Amz-Signature=52b5fdd66b6e7f3266f75d9b77746192f0304c0003a50148ffa1bb8c10b5f18f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

