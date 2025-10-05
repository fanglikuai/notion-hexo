---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XXPIUOXY%2F20251005%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251005T050049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEND%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCgiMnUogbzAOHsuAvbty6Rwg1aIo68zPlKWzKX8%2BAk4gIgRBuwrO0y7eIeL2w6hOR0Rk1ML4nn0fVVPWCBFyv2Bscq%2FwMIaRAAGgw2Mzc0MjMxODM4MDUiDH7DK%2BhFpQeX1vZoASrcA%2FwS14W23a4wIFzt1nitQXDu9UqDCCVlg2LsRUZ3iPDkLqQB5ZeVb8Ov6BiJJZFpnpdNhMYhNN5f8s%2F6yWHtw7Cb3eN2cuSSjC6iGCbjifge3YqHT%2FAwkuwWqxqU0tYNBNtMzc5KkMbyuwjIyffXH%2FwSnPTh%2F899HT2vO9iWdiOCpuHgnUG7a8ykNHpSVPUtVLqCvSNRIcon8O1G4fmlbUyLJEbwcvg%2Fr9V%2BAE1Kb5MQN%2Fugw2iPkHDrnrl%2FiuKx59KtGbr4T7%2FT6XsWwXKoaUoX3V2PoJziZiHnniqVRBAQQYO3O09Elm%2BvbmbUF9HvqVFvVSOyc1rPbZlrAJ2yiSFuSXcc7UbxrZqyjj6Kz3drOLXdN1v4IEmUJfp%2FVPqwPBCgk1i00%2BTJq6DUiFb3VaHaYtL8UQ2zFZkB3wEguzUFKJP1kENzPChGMCu5MZl11bgbAzqhE8%2B4u3Zkmd12MUHUfEB4IBJASZYYIqncOWpbFiiASBE5xoUSaRcGj2BIGaGzwUKvlRbxqowazL0YdOpKl7UTZ2rz5vWvSTRCtXx2ykFPk%2BBxsR%2BnzuI%2F%2B5O5MSA5JwZXCl%2F%2BHi8z8o6kwa3K6%2B9DLpRTOqcLKL2FxWOWOVzyPndg7uGTHrwZMO7hhscGOqUBpZ6wR6FVk9HnntZX2Mtkxcl87JzyTFGZiT45mWe4LQhzd4x%2FmzmlosFqqm9%2FhnC8LoeJWgiZiHr4Kfxf8LUf5WWZgttC2gM69TJTTpoXUBH848j1q%2BZVJbEIZNrbdIEQLM3NFZDNR3IlWQTEvI32gB5moPqx8Ugwuff4GuDlIHRL3%2FhoBjbm6DPjgfrEsOfXv5KJ4o6%2FdM3WiUM6fzk4tnMu3Eo0&X-Amz-Signature=369affcb079393dc03b410b255ab81f4f5dc8274b3f195ebca9d5442f7f29e4b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

