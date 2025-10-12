---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XCA5TEWS%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T070054Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHwaCXVzLXdlc3QtMiJHMEUCIQCLtTghYexVFTC0GKT2twnI5wnrJnR%2Bh82f7ol9r9XWPAIgZwrQCPJh1y%2FyUUSsX6Gi96pebCj0zO52al%2Bmgoh5zMoq%2FwMIJRAAGgw2Mzc0MjMxODM4MDUiDD4SmOmzoXdPUmaurircA4paVpLbjEN9iMXO12kKYaxdGDOOKZWgtPpu5OjiUlEeVS%2B3Gkssut4FtPVXlKQ%2FGEua7RPLbWu3zZ5%2FKGe66Cnj3jH3I7d7T8ofAOmvqhFesNHrAPDbD7WR2tir5%2BBwFS%2F1y0ZEC%2BLWParSBrzyEJl4aXECzpRFNVzIW%2Fy0X4OV3PRwf5EmOjvShuGUDezu1UQ3Pzsp5N01jnb78NvlQ3MQOx2gzbmrXCB4Cw0m2jzro%2FNNq0r3rWzhRgU%2Bosg9OgvxJmmDP84pwl%2BpKr0hNoftqjPVeN%2BUhm%2BzSWOhJhTXc4jn3Xbws%2BTvxEnZ51lXQDkI4Vl2MVA0%2BlqNXJLxTZ%2BlPkudLqOV0pgcPu89xHxN%2FqRhu6j3nNRHybp2NQHaOtistLNpAAzCk0zSE66qAdPcIx76iXp9TRcndvAELsCs%2BqZHglqr8DThpGBKhj0sSnnWg7qOg0ERIrBR3JV8j9obo0FsEyvW4qOEVDJqzgiRWBfJWvB8SMHjgvl9I%2B1PvxtM%2B1FABfVVXVe4cLxZWBQR6bC39C%2BYLk9GdhtSmcU175yU9usgobYjlBSL6gP2BioShA9IceZeTr6cwhpLnCDqo6WAITmj64l0A7GoVEbDZ6Zyxjr0sKuWOHHdMNTDrMcGOqUBeoHJ0HaXoirPvIFar7XuZkWrqu10L7DiVUGA6JfmIi6mxYa4IJXyxb1BU31cpzzA3CkpNr%2F7Z4gCSuOanN4KAtzwTKmeHTvMgTGejBE16%2BrfbTN%2FPzB0%2F%2FgMwC%2FzS6HAXVq4VgnYyjR4rlsHjwX1%2Fu%2B6KQQvqmUPP3vIU6vLvwj9ISQvVdBe64B%2ByDT13rCui3H6eBvAl773R%2FWy%2FIQwAAFZkybQ&X-Amz-Signature=0b3236b9c8d0bc65e40247f655f096df8eb49b8570d9922c75bfe68e73e9fb04&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

