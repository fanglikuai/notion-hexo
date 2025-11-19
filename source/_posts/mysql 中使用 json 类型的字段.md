---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666KV3HOKX%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T030040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAsaCXVzLXdlc3QtMiJGMEQCIH3QdDxT6Y8u782F0Z4%2BtEo3R8ECwkQ9drSwx5mp9tvLAiA5Hz6hc%2BUJzg%2BmmuIFlIvUrxLKgEz1nIcLnFBfCFpM5CqIBAjT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMP7oj77JTNHFqALu7KtwDl9qIRux2BGR2TsTKVS2prR8ZjHJEDszhS05V4q0MYDPznRiD5DBPsEWkyoxJ7feQt5niMqHuBya%2FESplQsUEVQP6w6DdnmB2F7cNCQzQwKHrdlBdFyFJyfqhnUfVTriGTsA9vZ4s9WQCawgbHSA7LzpkluXmC3x%2BsOG9lnCmtkTx%2FXNmkBapCsrjwF7dvN0BuXzWTdiiUQ6AmlKxc3571JV4iyXR7S7F3bvfs4K%2Fe81zdDfWdXHjYB15Ny7Sk851CyyYuXpVqKZgnrI%2FVvJ2MS1RYjnLL9T1ahCoxaZPv2oX1vphdboowFo0B55T2nWZKPvfFPx7t6L5j1MVA3jO1abJWPY9%2FFkEKh4KGDUbFFtxk63g2atLHQjItnkm1WXa7uDJ3MBeVCTT5Fg7rNXs4L%2FcnWMzrvUg6Ho8mXQ1eyY9qyGEP3c2d70NV4WXJaQx95Di%2BCqpIEvmZu6St4trQ%2Fq7r2fiykq9CMmtZrY20rWYwo3sIt6cjZpiYb3SJD8PUqv5s4zqyORPaE%2BsPSyvV0oHcTicGg42YC4ChPuf9VLF9QBIkpwISAFpJS6%2BChSdPDZAt2MT9y%2FNcTd%2F%2BWIhHqGC8DhzoerxK%2FO7bp1iEpBi8Eh88T6wIPrvoCUw79T0yAY6pgHmhKzap%2FNpBMJAvjeWP%2Besldti%2B7cvvyosZKP%2BJDScdHwIsVDXXJOCdDoBefKo9n2WvCqeTUCBfzi4yVYDK7MaFx0HWPCLLdM06MrXFJljXbepWFMXdrlmuLj5x0MNgw9jyRn29H3kQIyx1D3yFxC%2FHoHVFDMEz7ptGmowLPnwKRW3JtRVS7A%2F9AKkt11bZlDsmgKyoYFdYPzYYzOmwvWPck9OlO7%2F&X-Amz-Signature=02aa05cc3d6380a54a4d2d2f7208dd50dacf11e96a88e37e8c4a9c01e0f42f32&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

