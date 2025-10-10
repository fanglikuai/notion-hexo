---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466R2X2K5HX%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T180042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJHMEUCIGFurbuZL%2BpD%2Bd2FrpoIxnB4jE1wRcr6m6nA3PUV2xR4AiEAp5aGftoSTuNZEAxI4N4Y2wYz04BFkdj0VIpK6KmAmsgqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF8MQeGO9LFjPS1myCrcA92Zaq22WxuEpmDSSQsDsB8Jfsl%2BFsvLOiBAWWoFaeTHfgyVCR%2FutHubDL6LmElEVAhBg97qcPRI7v%2FR2AobNoOVqbr2G2lYdzdUFkBPgyEtULLo196pecU5aNlBUzmU6AS%2BuGixz5PElgdYbqMytsR2sGCnY6FZb6ag3HEuG3%2B2z1422vEWv3MwXHZmbnAY6O%2Frqag%2BCLdd%2F5mat2vBEyIweQvs77chJ9iKw3P4joG8xMHs2oPRceKleMbTRIMV%2BJN1bmE09mzzoCQXpPFZZ%2FjR4jmDZLOVgP5hZWG4ARaPkKYwEZ8yrAsD4gSjx4hS%2BtV397XAx4YosUom7fL2PcitNkBLOGd3BPS5U6QLF1KmNB4fzPjVmvdrH6WnRrc1rOi0ACn2S84XC9rn6znPra%2F6Uhqj8KUKXv8B0QDdniacs6vcBgVEfpxZ4Gz3Pm20ZXUI%2FjGeEC0Mb42bEfF3fvxmCWqcCSLZXZClozmajF%2Bl5KnKyOZc%2FEJdgbFxR2ysvSfxI1mo4VtrbO9hRMCrrncN97yg89590UKAzXB5jf%2FSnyPELEGwWi4ccUa18mC7lMcGjBCF7gVzMKlHErPLHipeKGofYLiePNudCuV6RMwyh24r5rCc47mbpjaxMPj6pMcGOqUBwFt73uiYCh%2BrI5LeZxuOnTCoaqther5JdKijImB8QRWbi%2FoM9KsGGeFt7GkjIWXrseR1ZQbYRJyH1259ShrYmYLOhFaWoNaQ90KJeS9S%2Bm21kPM0TJQxl%2FYOkb6137lLEDYV0IivGZjNqokopmv1Ntq0bnZGS42OUfxVwNLxRU0IBa%2FRvyOSTI27DCrHJCfLPxUdU0l305GC3HrJPdKFpvho%2BKrk&X-Amz-Signature=a128193abc8b878e228556aff59666aef44ad8d66382526898c4edb2abcf3af8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

