---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YJJM67SI%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T200039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEQaCXVzLXdlc3QtMiJHMEUCIQD7R4T1Gy4Ng%2BBFdxibtQjzwhCJVIlKU8hCIK3dMAAT%2FgIgeJzQ7ptmZ4Qc0%2Ffiw4HM1J%2FhxJlRZ%2Bm%2F9YxrgghX3SgqiAQI3f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDbCgEVmda2faL4PdSrcA5RpQFQgWOopwbwij0D0l4y9WKhq%2Bg64ztzLPnZP1SZ9QkNUfdglpMsKgNi8llPKoIqbMVo8fmk%2FGQTtKvqbo6yeWtxVPSnQV9xk6IgxPFZgCt9MjMOYkIThTwGUtoJ4EOqlgAeSdNJmdElX79fd1GB0dhqLSDOlqJ6MN4kf5OU2kkitn0PMOxxn5uRub7m8F1aLxu4e9vKmf6mRI51xxcbw3WKhnVmWBCFU%2BxU943bmNqBDB%2BeYlUN%2BvMVg5wUcxhf2mObAkM0uuImbuSIqxosq40%2BO7tTMTNWFx4MrhAQVCNxxyA576z27uSFGMUVu2eqqHaB%2FaG8ZtnEwhBLHOpkdtApC7qmKnuUcAtrCGy6xEBisXg%2BOoecA48oE%2FBKB7m7pRtGYh%2Bpy1hdD3XXEItVz79CdEs%2FJA%2FPLxdWNe9tp3Ey0hkjwIA2dBXCy7WVH7ztomJus93T8K6jD8pgVt86ayzwf4VLYzIZT4IAmx8fyeID2PhlY7fgyA3e8UG5nVeE86LC9MIMhjsibCitCC3ZhhqR8AAK1REClAulT4oF%2FgakyijVaX%2FZFW1yUbwNwEv0sztUU1BpNbvanLH7Yw5YYUFlwt%2FsbrEOZog2%2F9%2FuKI6kdbtEbYK%2Ftkr%2BrMNWkoMcGOqUBL6Kdxdb5uSAJm4G7WH4ortm%2BdHFxG0Kj9BGpfu1rSZofVG68WePFe7f7bMyvJmOjFJlNBokT9RX2gAz8r%2BN1PxzYTj%2BEM9Ih%2B8ldHrlNXKSanpwIkMURHqJQfy%2Fs%2BDfbMcnmpEo0ux7wfO90hnyGod%2FSU1pM1glYxoJvsz7EC%2BLHBxDA8qprdT8Njl%2BOquCJaCo%2Bw3hdzI1M0u104vfKQk9Mg%2Fw8&X-Amz-Signature=97f6cee7fa0e617f8e3c86f14492e63c2561d9803a510732632893445eed36eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

