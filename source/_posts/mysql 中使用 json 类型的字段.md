---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XLD5FNHD%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJIMEYCIQDEkkw7tyWUE%2BJCkO7aFhMpfEHL8D0UF3Kt%2Ft6rJdwCFQIhAORs79g3NnktSULK7jCtBsPrZtM47ZFTzPtz8gGtdDJLKv8DCB8QABoMNjM3NDIzMTgzODA1IgwiA%2BymixOuBieeC28q3AMLqhoTNXcbr6sQ07HSqCMyX%2FDmR3pkmQGeO4V35Xr%2FRJxwLrV80GBfHg92I%2FEw7hlBDE%2BiotlM8BW5QtT0zpaAEI8C1JpoCQ0OEFSYc7kU0eOv5%2BuY4evXENbgZdMDpbkVLq9IEvG%2BZJZrN2j2inJqdif6%2BI%2FLge%2BSzXmAeYCt8g3vS5XEOFtYV2jHgHl2Rjlejg%2Bqm7yVbdVFkZ2CSPrRCzg575JmeMNyIu2y8sKMtZexaOE4xO2Z68CNO1HPuKX8Z5QqkKhXGeeWkm9bPU2k6wVcp2vxmfCexU0GMNDYDpt3rsQBXLJtXtvL%2Bv74UFLOTv7HHw%2FlnRwB5X3Qzj6GPBid8T5qftZ5ROr%2B8atkJjsmujGqJGFVtnKPJi7XfH0v%2BxGAnj7VdzwKwZFzX3HmTHaqxkGNAGnl2IPXTYvjaDJz%2FCPJ6rb0nojIZOqrPaljaDb5ROYCAQT0SBVpTmfMTz66sPOGmcHa0PN85BZeHI1riDYzmxIlZBAEctbY35%2Bp8MnPihNk%2FvoSY6a5CQhnurmswKYiVIszLby5CYET4g97hvuHuOvAuXHOU1VJy65sQLOVq2jQ%2FyGGYHUOblrGUsBBEUbxL6vW%2F2v8K%2F2S6a969clNCo3E9RrRRTCl9d%2FHBjqkAZPUxG5Vynh6J%2Bd0GyT%2BtEX6oFEYWegL2py0qd6B3optsUbBEhTX0RfSCbKEwBJveCa144bgBOp5SwhhAjb4zOQgjkQvjZ6B69IEr3ZvlCNLdjT939VorKA4FiK52L9EV0tQzliJhgkenvanv25wIpkceG28dh%2FecYxrLmzT%2BmfVvm%2FrwqcNoGHHcPX%2FBN%2BNaVfk5SPZWeTs%2FuA4%2BA%2B2IpagFOZU&X-Amz-Signature=7e3b7eacd015d59de87fad0761508ef1ce6b0f46d187bbbe23cf6a3c5262494f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

