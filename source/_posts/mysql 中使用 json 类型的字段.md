---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RY4AGNCH%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T150057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJIMEYCIQDM0cKgagPlwgdD41DUAhn7Xv%2FyCWKRinlNR1dmW3yyqgIhAM5EdB%2BiOiv8U5I7cyDIkQqKzwG07DA%2BBUWRjVujis8cKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyBVKpvQmScEc5GLdoq3AMukrcLy%2Ff%2BZNHJt9EX9Wgml3T7y6jKNFJUmM9LZgBZ4mvsO6bYKOlsSuy9r%2Bg8Y4ZIdaO6oQjFWsLEbcQzVijqmLgApZXoMokyG%2F2L%2B%2BaeKKLPyQvQGgXplyB2Lembe%2BgkQu4WABmWCsGiPG9%2BGZ%2BaOQ%2BhZb03RLYRRDB79vxlnHTIhgsY06GN8D648%2FEUl1t23ownLaQTHO1iHHhecy436dCHTRwc3zFX6ieIy7WL4czMJKYjALBZ45mNVW9jMshL6ksjULi0aUWIFdxfTxlyGr%2BLDkKGNcsvafu6SmZnfVQwZxR8p8xB43xmaLBlMw9HpHbD0Wm3%2FjAmop1EaTPuZe329O%2BA52BjCTctwferj647%2FJ%2FfzfPoMaxE4RggN2PWRfpUkPXH1PHM3OLYVCc494eUkZni5F%2F7OiuR5As1kqKkfJPvciFXu7LgyVfB72PZx96Hd9LSI02rCKW3K%2FvG4uUHve4nNz6wFX9tOHz3dwHXiaYkfxz38E2ua5bSEmy7XfSqXseUuY4yC1p2K9o0rHg7p6iEcUXZ0IoecDGqWtY1c7ffrjfyV2cwClA1jx1rN8aadzHJL9pt8uEg1wmVBT4nIcZLrxZ6oW8kMAmYjar8bkNeyM3%2FkqL02zDQis7HBjqkAW3fwES0DmHqmqk90fQu5SkcXfHIAF%2B4dg9Tno5WDIlnq7PURullB1V%2FOp7UFuupejd4xdNFAjRqkAU66vzjnhDKCglRZpNShm2SqAFPoKV23bWbPzn4Yh5Ped0dXFVfZi71sLWDV7YR9GFseemyJ2pN1w9VcAsTniTX1FR%2BZ5QP5SucwUOpeYkyDUy3hhpf6Pz1mEussvmQ7EaFs%2BY7iVLxlqs6&X-Amz-Signature=5e8faccfc0a01fe7218c20a44d20430367e5d04d2e046c5ba75967f0e569637b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

