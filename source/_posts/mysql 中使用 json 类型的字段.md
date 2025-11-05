---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UCIPOSRH%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T130052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCyEv6aAVkkZ1WH3WgJS%2BJuSj9JMATFoOsOSrevmdjjagIgekUQhpvuc3ty7oAhTaIYkXIadvvhiS6QiTMCGIHS%2FTIqiAQIjv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDKgrkua09hHgwnWlgSrcA586USsXUfEvmvRKfUoma1E6pT0GT3I1eaB%2Fetbp7MHkmpkGCEKGyQq6FJRRlzE0Cq0Z0XRBaqsfZtN5gyjxVlxjWRslB5mNASAURQhQKeGbBhidbMz1hpE34H9zckNZ5CbgnCfuXwVpnl0SIwIuv4E%2FUs2kmYDQW%2BOykztTaUBxbdJ%2BoNXu2ZtngqjXNr3zPWKsEqo98hK8Ky7k7Ih%2FvbhWLj44%2BcpROCUd%2Fs08vwRBFnioHT5tTKYHWz%2BIhxoBDVxpHjRWlh3e%2BJ1vAc0tWUvZGvrD5Ie%2F96CDL6p%2B%2FthTNFDAV5FHbfsiJT%2B2DXPcP366V20DfzTWZHra3t2xQjmS773iqrFgUeQNi1pdQiAcQ2P1Clc%2BthMk%2BQ2V2Jvn3TBVTHla2D7ws3%2FR0Jjl5UVIBWrM92QXsodJbBHKmrCCrxLJULwWDdV5jQL%2BAhWhULIswJLrLZClkCO27U4Z5zL5cfgZmCtwkEdT6fClQlz1mZb1pmpeJw0IK0dt0TAmJ4EFE4fBPgOE3xOUlRFN3kjBc2r3OT6601Qf%2FPpID5W5K0ctxiYZ45AAqgQtnJwFx0s%2Fg7G18Hv8V%2FtsntBkcQUS6p4VbyxJjAsZMJckWkcIZ0IVboFV6FXnPmdDMOmTrcgGOqUB0tDWKdBANPNYUrMuzM%2BZ6JXmkGE965%2FpAKWh%2Fn%2Btir%2B1C%2F%2Bl9H%2FHMtcnfW5SNgl6SKsUdtokXng3XyEWqGBCV1%2F%2Flz2DjfzsNBTIQuaaq%2FAG4%2B%2BEmtzl8rZJZBF5PdPYGr8ZdekXd9g2UdkkO9DK3dAY74zwKXdRDFOQR69G0Mnp%2F2KHx%2FZyJ3cwxALM81mOw02GDBaNUFmssXzsMfRYyTGi%2BBee&X-Amz-Signature=32bcd6ed9e18776889f291708d5d6a76497c48da9bc255e8a1ecde29ce48e971&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

