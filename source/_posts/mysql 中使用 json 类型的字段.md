---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WL5H3DXQ%2F20251012%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251012T100049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA%2FhXD5hdse0RjU7EjmrxRyezh6RDOTteTbbLzw5Va%2BiAiA9tGCW97zPF2LnBV%2FCFtRZUkr5Ki%2FEc%2BMZoKUGZQQ%2FKSr%2FAwgqEAAaDDYzNzQyMzE4MzgwNSIMM%2BXhtn6EwrDD%2BnY0KtwDTfHFb%2Fur6CbRWJRYkyuBdowrPCYD5jRagCDfChhjyEcvsISMwpdXnRzgrOp1f%2FYyJdJvpnULdoa7TXu5OKjDf3DWgoY1%2BqLwSoYjf2V%2FmHZSU6JvUjG7Np6jtwEDORomNU35qjLvxZhz05PXhwromEVZoIVhQeOBk0Cc0Ndi452%2FDH%2FD%2Bk0UYprDFLklrEJnrIYSOgnq8eitvbA385vmM%2FRExEuDOfmvmSscKMesTo9IAQn4y3lHEPQap18Dez5cl2iF2ypxIWmGjDG020Jv8696ueivMRiAYDkIjy5ywqYc3vRsNKxUSm%2FUokiTnkAGJFHib%2FhIHptBAVOLN0FLzLXogG32JY%2BdoMZTVYM%2Fb%2FnOsR1eEGtIrxOgkne98K6oNnNmrLMvJFYVeo6WP25LyOa5BBEEEKnSrMP6G7H5ogXALJEZiRhoEgqfAS3mH61yl1%2BZ3VCpMHFCnQnHcLgsc%2Fm51nF0XWCGc6rUsQYw%2BQPGTh2DMJOEJLu4UrT%2FSwToNEBx3PKO%2FRDDegv4vyIbD%2By4DM%2FXPK70wyYxKrNlJV4sWgk84AGh33xiYSwZfnhEyag%2B8XZ73%2F6zUjqJCxEFigs9MGxTr7zw%2Fi61yA2PraPTjI%2BTJ6fSD1GD6OQwneWtxwY6pgHOFi2IzJOpxEo4ftGNG0FIgEOQJhOjc%2BDCq6YZcKFMEiL3%2BlNgmPMQk7yCpKaMKQs3O%2Btj80zHGgrCQnFGohgGk3fKSiqLbpUddETuz4wZK%2FoIpUSPD0Q05FXFknA93whWLcfA8mAvsNHCfAtP0gKk5PRdF6dscsfdK5Kax5vp8fcoawjX7MNLpvJ1t3y1x2AyYJbZFsMcxMq5mGvhwKiUMd%2FOuJ7v&X-Amz-Signature=65fe7427f90f7edb29af2b25d8608793e45d05223fac609db5aaab8a487f3c36&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

