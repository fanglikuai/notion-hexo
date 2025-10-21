---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663QPREA7U%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T160048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGAaCXVzLXdlc3QtMiJIMEYCIQCn2qdos8vrKwJS48i0auyDltTFE%2FvqucL2cGnKFgMnUQIhANJlTMhlkGCuAilqzxcue3hBVoOsL%2FmZRb5Tj3e7pFmpKv8DCBkQABoMNjM3NDIzMTgzODA1IgwMEujScjVKC2sYMJ4q3AONXymAKiahWxvT2MOty%2FTwcgPS%2BM49tE9bDiITml%2BA5jTZGbxoxztOK3%2FMFj7s0vD%2BhqKSWLsKJhHhDIq6gonKqLEIboLFO5nBL6Cbi3LcSTMMD49qG0mI2BWlzlagW6afP5%2BceJwcAVGc66Oy2fc1TjVrB2yJzBvNiM%2Byg159JDK4ZTILTfSnVUMvN4GVWA5sP3GKfXJzysHWCGKQTAx3dMRCn6h%2BCJ0Uyk4Og8qeJ2drCWXyoXV2SL4wqBvj%2BuloUUUmuDO2PZQtGFZTz37fawz%2Bc7IAMwXATA46Z8o%2B84yWXG4f%2BG%2FF%2Bkt9axPLI7OQc7H6D%2FlQ0Q5jJciTVK70UHnKcF9lSku%2BzaeV2%2BgYFgst6anEHRbW93cOFrZsDSusTghYLnPPEzqZTT9HRFDHbpZTpLYVBou7C3SvzDm%2FeZ5OshIB8rkEx6r5yFosWngPVoTg26DIk4aoT480utxMXnKrmDg430p8rZIXM25J3Ifmox17JajOjYII6kR%2FHd0n%2FZpkZDBxgbfoxLwMcZeHnlBW1jACDbZaExfU%2BmqyqqTzLBNq%2Ben1t1C5x7Uu4VzA7TxE8%2F%2FLArLUmxs5gAhiR9Oj18nLrrlFK3nVcvnllZeoHLuQV9LnrWXMITD70N7HBjqkAep87lsCtnKihgzbauAEyFfuLiDomOIBHsUUkmKJ4TAoF658In4P%2BqTuIv7dSAeJqTrLRGURshkFzwXPbsAMl6iuDxD2l6m7Gg7iUfBwoUEvvj%2F8S7ic4veQcuW3odtYfsTSRHpWmgvpIcyAlAfopxWF7eVOYuLmhjI%2BHR9KfOTsrmE%2FRxPRpf%2F2%2FMJM5F97n2i80h73r7VIfzmYYsKy3asRxOb3&X-Amz-Signature=8f07ba5ea7f3fbd7cf9053258f7135867cc4155c5ee80b3ac9c5479302f3196e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

