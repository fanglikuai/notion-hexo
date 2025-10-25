---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TJTWVJTW%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T030059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCvXaPglCbCqrsmUvlHqe7uZVQ4B54bsBj6aHVqIrkEhgIgd4G2R1JzOFU0NEMx1R2miCJ1QoeZ3iRiBJ344gR42sQq%2FwMIbBAAGgw2Mzc0MjMxODM4MDUiDHIMq0ZxD1QU0o8MyircA4T5B30cQ4u8bp8FnLAvlsx6Y8OdY5evLGh4jlxRVlelDSidWCf3HFWGVw60P3HejrVhgRUtVWJ39RTK2vxpBIHqgppV%2BrLtmd58zhBUnBwFoNt%2BPvZ0xHMk8RNwnj4Q6CyI7zzw9LXnBl94HgaaP11eddBlkM2gTvc%2Bf9%2BTBnCJs2CfzRqapyMmh3pPe0U2nUTxnLrB4vxghyGRQbwjSqa6ZZMltpRpfgxd1Y5y5xIc7sr4CKTWHaT9KUc88gaDVnDVDfH8G1LzM4xVq7wS2NPiGsQwZzAT0O7EdK7Ngm%2F0Y4Ecmk%2BBkUElDYkfp8Hl7Y%2F7J%2BepxtLh9BpilnVixya1RTVFRL%2BjK0Gt7EKmnsOKZrNhQ2nfeDcOJ1vwKksfNuFpkDa88KX1img7jTZcoV4GWSnQHcRlbpmCbwPE7paVvRiubu7JI5mOQNFsqp3gXwLeWRd8C5N35gWdjNmrzfmbewlkAhLeStYy3qR278EdtJ4V6vl%2BjpEIociM9KRICxMo0%2Fd%2Bs6IN8CLs4Sado%2Fis%2FqHR8POOVDv1eh9yfoybTo004B%2FGXyciU%2B4r0oT4w2niNBNx48OTH3%2BZfCG8qnNX34Asbe%2B1HsDoxrG4F%2ByKWd86hFbr4h29hLANMIvu8McGOqUB8LL8E29b7yJaQ33m12f221Dm5BasJhmNIB3zrc2AGQIRh8FTbGo9kZYX0yfLC4x90Qdc0AXIhTzLUrGBs8%2BA59BHM3%2BwSL0uVTs1sTSNKPuCWmj3YHberI%2FJN3ycXLUxeJcBPu%2BOQ2BLXn3ZMf1hzeUDfh9dPcvN%2BtuzA7EvqwuLOiTexNj1EZ%2BT%2B4MnGaiNotWXr%2FSgCAj1onuIJHiZWewB0fss&X-Amz-Signature=a9099fa71d707a37e6a017cda627c3fecb9985e3143afa3e741cc3167482e508&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

