---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662ID4NYYX%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T200046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCICRGrzdtRQ3cuVeR9kB1e2XCe00rCCEmCP4BQ4vzQr93AiA%2BSVyzSTFfzn%2B0yFTt6WfPF0LS2p82AiE00m%2ByV737YSr%2FAwg1EAAaDDYzNzQyMzE4MzgwNSIMsgwg%2BVObsq4Whbz7KtwDZMbzCXgs30N2wbEuw%2B2oXnpmdxwFl%2FSEuO1Iwh12q42TSY7M6vImv36OxW39MRwpPNc4nr21ScAwYSbhzMMy5VxGPQ7SeLZ%2Br4r8H9N4%2BehxH7t6Jnta2pS6N5JNBW8%2BPYNrd91fDjSjvLzKzUwsiHMr4GofxeGhx94VRvWYU3BV4DY%2FE9zlVFsHp1fgLi71Am44BZA6pPcr1x7lVFcULdzp4LsCCMccI6t%2FYvvbj0Emd8XJeacJzqWzOPGIzb%2B%2BusteucvVFqI6MpsEJF%2FEpTw3SZlo%2FVXB5urC73Zot7IEeDYMLFaMLlZl6nt25Gr7p58zxPV06aeRIQEjNlZviurwlsP4r%2FG4iMPItmCFYqL5UAA0jLhm5zx%2B86kVV6Ed46rZ8%2FTiIBjxpirsrGIjQLNGdtyV%2FaaqDVBjGPTraXuwfZduQMBCIJ1LzIIVapJhsWKj3qr4N1%2BvdYvC5zkZEmwcyCN2TBp7NqoHqx5NHB2cHiNj8UHt3B2nWuSsZbcNCe6k7w9EISr%2BpESB%2FrClK%2BwwAEGdVsZtoI16ujmxz%2BDRUL%2FOrmIkN0JvI4E0Os%2BEMFbKlY9mdhjtskBcSxTkeQHp%2Fm5cOXgb13eEZimANyl2RRfQh%2FgngozX9p4wgdTGxgY6pgGo5iprjS7wzrrNABXneumMtH15snED3HbJ5D18PhbzXXS272yKJCDZifx3gRIyzJ2P%2FSqSIbR5%2B1ICUCJClHQ7MsePDlu5WjkVGTSjv3C4FQc6PBWNmGQztT0g5ZKmEFxxDWDFzyg%2FBCKUZcz4jOcdBjaWdEXkOBgYrOPwpc9P2%2F623r86qBXr9oV8AEK3jWZ5YfsEcvRvPoTISeFtQI0WXolqNBQ2&X-Amz-Signature=e7702cbd4c0b33aef549a1c9acd3548307d88a4651863958a8c6f288ff781611&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

