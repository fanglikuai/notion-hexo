---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VF7CMSC5%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJHMEUCIEXGsgrS7U3j53QIHY0FXHR6oUTYBedrbZxTAnWat9MPAiEAjPfDzCARxtX2WFVL3IfR6wVCvacPLRyHmyKO2BYrGg0qiAQI2P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDI5BT8tIHjXFQRiXYyrcA6VPgBeOab2rGPQ9KsqqVH4ilmYNNJ8ZOWNmLpWEdAeQr2MDZlPdoA5zaWpTOHjrBJmC5ZaCHdHCdAqM%2Fl134AAqAQeOMFuaeNiu8hXJ1IXh9IbQa6eG22guzfNZnDwnodyT0Qcd9QqNhTZIqBfrcIEDxZyRvo7aCsdLN2ILOFwsf0oo3SydYNsFqBZ5xDxVF3vDA49%2BnXd1y7P71VeDw5X95PAUgQJjlVDgmyWFfwMiXc38D08A0RPGTIRExV0wyKaS9dcg682lh25EgLqAIbsFIWPV3lSuYRHs%2FXRRLepKRNXBEuj3ISd02W9Dsc68%2FS60bzdBqhYSX4tcHbxHESw9dP5oMjp%2BZwTIpJJJYowW21Q9C%2BtM%2F3SUr2SLp7e%2Fip%2FLiJaeTjkH%2B6BoI%2F7zHJPegQYcgeIvwwLPrf7nMh2S6GByDz4zusNcva8sCn3e%2BSiVamzHMQsBgMBElwGeAGzGThnu3g6019voC3WtqYGNIm2kOXC6rZ%2Byip8qQNGcd%2FiIcN1%2BkMCLF%2BS9JmBgGhfhENBxn3AUJrwFSLZJYksAlwoFBnIOnhLJRbl%2BMYBhCA%2BouAj1onZSbRvd8on93u3Sus71%2F63VVdKsiwoGc0IbpyofmikBfMeEUDQRMPPTiMgGOqUBj8ca4kbdElawkmO%2Ff0eyCSRiL5eawlAJvejUxEWW6nwREiYgWSu4fazCmxbFWBPxRGUKT9yyPuXdThuauRTGS4JM3zH6mTsBANj%2B2A7RqNV2zCDb5r1Q2mvC66eZPR%2B60hFJbJioIbgaglgqaYSvp6Uf6l3EKPkmDnwOVy0la7QdkcAOIPoSn%2F%2F%2BlY7MonU9lie8nzmFNLjeIBsXwKWqGmA0snbL&X-Amz-Signature=2770aa81b07993c2805deacb5bc254b381143f5266fd756a0b7a74e8ac2b6ec7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

