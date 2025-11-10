---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666FZ7RX6C%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T050043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDQaCXVzLXdlc3QtMiJGMEQCIAMfjh6ohDTOk36pV03nRlX%2FVebLuvYYsyDMGDS7vNgcAiBR5XREhrljwOatsA0uBciucJGLYmSIS5X31epdoaHC8yqIBAj9%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM3p%2Fk8au7bhh886UxKtwDH7tkjb%2BgXNBc4JDHHSdYMYtUvjnv2Y48P1OZrHgV9z32BieFZYvZQSQxwK%2F8QaxMO4nRMAH02K6qgpwIbS%2FI5kS93THlUZu28G%2F5BVgeIfHA84EI2fxA1QnQNjdDpF86H1y4ozEAD1D%2FE9F8xEcehPh7CZLKFysAVtPJPn9GhjNKKPI42nlw2dQ3FNbboCW0RhLdZfgwXvWaf4Aao773p%2FZtX6vCumKbnqNU4JHbyv4WzI76gUs7bf8gDE%2FtPBYN3LPA%2BbBZHZNdm7Pph5qJdgvzsZ75G9HiBSicX4mRc7ydc8S8VLCGMDC8zOhEFf3xsV2ZGG1zevI3tq2GIaqN%2BY09%2FgRz3AmPSdOnO1T6jWi3Xst2jXyfxwXhGEG%2FBcXDFhm%2Fm9L39EATQQn0%2BlKcYF%2BGZWZq6hltrQwvcfypjXrw0YiLqQP5mG%2FGWvZ2sMs7PQ31gWPAL4gRrUJUSNYXyd1g2EwIf1FgK3mGsweu85%2FJbilIA2yOTksnXtl7qUjyFhnHUikjEcc3U9LhQGtPgxCr11FOxD1xg5G2aHOQQPhgkdcY7dST90UZmYEIPMZm7YPflitb1GA1pqjC0dZ5walXi3Or88RKMZt%2B%2F3hqlD6SnjAeUpEp0NZXvpswprnFyAY6pgGh4%2FBBDSo1aUT75p5y8GLxsUaNvDWLf9IjyTw5cBpo8uvR5YJ02kLW9x8xtT%2FM6PWOhVTgKHtoynnJrgscXYk0DDKQRTqmC9ulAj%2F6EZwoCfwnyb1YL8%2BF2NAQ7LuiyuEE8zI6nt3CDj0Vp2jWRZ1x%2FUVsXFIy0fGjzm1qbC6Vwjru6Vl2azH%2BSZ49H4Zk7gaj%2BlZCB1hQx7%2FCzRaBOPJuaQnPFu5R&X-Amz-Signature=bc009a8fbd7ff1aecfbce911a5192c7e3fb76a0f4b67d6fa1fab7ed2669f8dc3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

