---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WPPTK5YM%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDoJhT9n0yx%2BhcSjJePQ2yyS6233m%2FLywg0qT2yz0ctrQIgf2lPDFybb9yTXFuyAJ9eG5%2B%2F1O8jqJ9OaUpd1VEoYjMq%2FwMIShAAGgw2Mzc0MjMxODM4MDUiDDMRamQE2iiM1Yk3vSrcA%2BldcexHcvAd8SG0oVKPugDcnsieM9OB24QZfKTVE8CoMXb8brCB3D9Nm1OEDuUauapdr2JS3G9K1x%2FxkiXba7YWc9xHpIcnlOWbHmsPkQO2DDKWCbHEVs7yWhp9EBmbF2Ryxyx6Pq9gZoo1CsBbyhf88JGonYsC9rD1DuJLlQEtbWsIpsHo5exLmoITq5eQ%2F215Df3KtdZAbD0DrLUN0p76pgZGpaemA551WWiZTU1gG1bGl9tKWrGMEWYZJ4wQjccrc3u%2Fmhl2lXRb4X0kzZXsD0KpakkGcSwvRAj1txVOkQje80lAkjsacqfu7ECGO3z%2BVtksNthQLEmD4PDSSULgU%2BQRbYddynDteSw4MvI0SSEdrkS1uzhsma5n6CR07SYwo0xTN2SiMqachdll%2F3vFWzkJvDfHKylx9p6N7zlnFSGDiXKdP7JM7kYJgU7%2B6%2FFs7cc7wj7ltE%2FNSaK4P5A%2FAcxlq7g%2F1RVUHID0UXGdqDva71tQrXqtNXfXcSRzcxTRuBFLESDPSUvezpHk3s0anGJWtKKsAr8l6rrOz8XhtT0RRXb8%2BXLc4vA5fckC8Uh0Z6poy%2FzgCJimD8jXpexB8MUzYMcjgy6sOMAdiTA%2B%2BJ0ecB3j6YgYuq5VMJ6by8YGOqUBa2fy%2B%2FHs%2Bj421RMkAZ9WR41nHCFB4Jvmy%2F1o1%2F%2F4C0FbIid5HkLmEwunrvedANKHEuYQQZhHvMNXsvBb115Ba%2BMcEaH%2FYnlz7IdcVUZZCMwRSCN6Z4JkvKJcFsc5a7sTQRXEcoIISaXFT3RcaGxv9H%2FNOHy62QEIhcRbz%2BVLzTzSrxthBG1ir8MiRRkVYuDaqsJ9xkiRK7Gp9k%2F5m2bjvlYbemOX&X-Amz-Signature=6d72e274286f99aaddfc1dad510ce30484c6f2f579965faa5e7302f20e070a9b&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

