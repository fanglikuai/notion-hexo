---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YKFY4Y4N%2F20251010%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251010T140049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJGMEQCIAQeQ3j27OTiwC0nldSCMpjdju56M%2Bvz%2FhDmBdOmowVTAiBJhEXE55QKLozFzPWWso7ADfKE8qMuwbs44uRjYE7NlyqIBAju%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMyTs4Rq1fhk6DpEVqKtwDSr34XYZrmTzLDcra7VDEkizkixxPKJBIzzDQOw%2BR5hfgpDbZKW1m3jXXUmDjsw7IllOiWe3DnXP1zOKywEmiN11HFYv5Fc1NdXqPjOZ6zSmNIDmlPDeFp%2FKx1pS3VpvTj96u4qXxeMpTBzNF0W62IHYq9iCmD8%2BsiQZZ13D24mQNusPobxiiacWdA6piOvNn8owAEtRvLw3HvAXd7%2FtrTyUBwszYZFjmgcQQPT8pwYUHSjtcMLOtbPjMq9mMim6Ja1oxQdVuMK0abrYH9ruTkDpwtNq%2BA5Mu5%2BgHPrMBPQaIus8a5s4%2BKm7bEeEbdTQ1jACkgYtXzlLFIwWCxgTMZgMEMTWKKq9TezthUBgrh54YMe%2Fkjxmu4J%2BxdrH2iZv0u0kDsQ3D31l6ntyPtwICFdR6MAWf058%2FChKCcUieH6%2B2cZyZsOz9GtKFMd74zF6z3kWXwEu1jQPDlW30TX%2FqOB5SSUditTyDAxHcYtkdc9vhizqzHjWN5WO3LXPywqTLHvOaycUi52if2eWRJUHVDyfXIi8TVFFN8haoCe2awhpQnsGmF%2FeW1jUNq1QpL6ip%2FvHgX9Ip2N2xnSK1eeAdjv9uk7futjkr1Fdx%2BDZ3QBos1wE%2BiemuKNDjFZswmYmkxwY6pgHQv7G8zDYZrNJkuIkuf3oL1lkzSGlLUtlPXu0MOtsLh64TOI8svLjNHD8zUoAyIoRsEaWg1o3YyZzjijRQEfhQkZ0LePhiyB35eMFakmzY3yo6kENs9uA9Ppz2%2Bsfrh%2F81dBcuK%2FeMuhjgaStjBTwcWfAW1FMhDSEFPxnbgQExyY%2BwUluryKPxAv0wek%2F0Rr2D9zn4he3%2FmJoFgbjORAiTmikUvEv%2B&X-Amz-Signature=4ee38e4b677563f69ffc842114afe168073f99cc39bd907614106307e5b0bfa9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

