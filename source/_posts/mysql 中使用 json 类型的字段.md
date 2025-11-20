---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666XRWQB2J%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T170046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDEaCXVzLXdlc3QtMiJIMEYCIQDIismzawN0PCxGIYuMiYKj4R2jNsnlnQub5y3Vyu7i3QIhAOeNXhWJLNbf7XAW2WKO68rCP5Uf%2BXRlSQdgMK5yOOCAKogECPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyYDxobUlp0rgxjGTgq3ANcpXrRnZ6ovNthTiS%2FJ%2BSqDvwvAi3nnEPLfSr3PxN2Rp7hjXtdJYeoa%2BByhJhOJaIrPMmfbUueZ6UA%2BDU62TT%2BFTzi4M8yKc3Mh6i4p1y%2FLyrJpchluNdN0gGU67VJ3WGvfgn208sLOeHaCJGwYsdhTjr%2FXIKuCEkjrMo%2FqKVr5MgYLw3VDhy6foxNcw9Pxb5akqL9EAKdWAAnPaZyZF4eufk2E30ETdxp4LImJHfk4WRaapnZjUB5lTPkOq8rAiVZecoRPHv8ZICAvM131nMyTa0QZHWxuItuwJPum%2FKWNWdVYhYK2i2ZHT1VAieaFJ2mBBzLu%2FrD6O%2FMTN%2FZwP8USJgCnF4ArrEyoI%2F0uOZJGdDMMefEg22ypVa2p6%2Bn2SyhNSPGp0pEyisiOH6b%2BUK6%2BavEgNt%2FWlyelRhSe3F6igq0nrUX40rIo08r%2FHgOAPYlnjPVE%2F3yF2GxRZZTCsnrxmRnm%2Bb7pKzn172EA7sAduUY%2FP%2Fo%2BdkumeCfYzZq06t7vi4lz%2FhJ2%2FQbbtq15UrzYJ1ZIJmvEh20vdBaF1973%2BSxFdUSVT8ZD7OgMIS1IUO5HScWZ7g%2BYt1lUKQTKk17mli3q2hE%2Bk7ZACck%2FXP%2F94msKpOeAMx856F%2F5zD2h%2F3IBjqkAWdzP29RX1MBdGfkef8CZod3fRWdKXBw2lPLi89gJIuKevC7KtbzJQ6jSHBomjGsaoPS4C%2BKeCvRiwo0jr9lhYB3MYVZ4W2zfLrdxNTMx7FvPIzgkMECgL6Cxj1p9NpLwp%2BDQlDM%2FLiqtCKGJ5SNL2bhBaKikbqVIlQaGO9Psaq9438j%2BPP40mux3%2FQH1lPCL7dbmOjq7xuUS36reDs8IIDSSSP%2F&X-Amz-Signature=e670b1adf3c772c28b9a2e017164ad9c3d3726cb5dd4c725d4be072abd5010dc&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

