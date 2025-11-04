---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W6ZICLNY%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T190045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDvFlrPUuv7xVGaFpCVzqNOia%2Ft1zrWdEPcso%2FgdqO6xAIgSLmetP8E5c7KrxP9%2Ftxaq6TX9RZ2pNA0En%2FCbTAa6TYq%2FwMIexAAGgw2Mzc0MjMxODM4MDUiDGqbwd3OdvtlplPxYSrcA8vPSajzN6Z5sdE6fdPpt0JhJ6XBlcpcylXy8FnvNaxJxCzOwuFpNLCdjrCx7D8g1AZapjyGYOitf09ZVUGI4dRcVTZ37cxghUZzLioRUaQTHqV7DBhmqS03ihGDR4KKdo4DJIEsu6BNiXBULcERrDM9yoCOaWdFoFhvymiWRVVYhzBWftcWQJ7X1pCrvYB7F6OBLYpGZ9EuBjcfxPFekD5vCXcF5SifRG31Aoh3rwz%2FeMAR2HT1rI%2FXNyzSBnXh1sOn0iSBcMvNH%2BciY7zczoaTxfZq8UXIr7seoy%2B5PlrQ2VgcyDV4XlaYVtpnOw9q890OzeQvoVUCUNKlfkMYMJ2A1nmkCQEo9xNQr4%2BfUiS11sUjuLaSCUI0rMKiuNNqbg2Di5XsRG%2Bf7nUuazt48HSyIzXHIDCJoysJCFy1tut4sxBTFo8wWvcs%2BDJtiiVsL96ExL%2FontwuK2eGNI873pBwkZF9Iw0thtUFfs%2Fo69q0iZvrVRmEd6jbXGSV%2FkQ7cPIkRz2HHNwAjzgdrWpUTu06zAiJXyT4CKttjrdQ2h%2Fp1%2FcVMSWLgru6UyJ4vpm9PQMLmBOeABIs7X9ZlTqceDSGaWq6ZV7slb6BjPrz7c9eQUrFWzXrIw1epLJAMMGAqcgGOqUBWSl0o9l9NSnOVAZTK2b9eI0QAndKrBKmr6YJwEDzmZMJYoxTYx2Nme%2BboMr2rcHnTFrm7ggR3k%2Fh4BvnyfZb755X5OGLcK46BKpA9D8acNtK1kl0psoSxrNR8tRRgkf8HzGD5%2B8lQ0IzXfUYAtf4KZJRD7bxE3MzN6vMmwuMsZuxnFtKXAhxUEajqkB7LVdmBBlaa1RXtLgTbHJfkd1DOs9bcLbU&X-Amz-Signature=08c2115cf13e769c9bce6cc1f1fddd2840ab6e5e8a8d8596870e65896d27c6fc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

