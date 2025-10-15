---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNXDKPJI%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T200042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjENT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIFzHRcAvAIOJZEaBG%2F%2BAu2H0BmKDF19GESDgpaiJyiY6AiEAwZ8cmM7Erz9kp64T1376WkD9y3NgR2q25teL01sRdvIq%2FwMIfRAAGgw2Mzc0MjMxODM4MDUiDEgMw4WbstlzoYjqTCrcA0dcdcG91BV8p1KgEsEApbiE96wlgheq0b9zjLCZ%2FdaUyffDaQuoihmnzO5Dz%2BRTHvHvkypffqkrW%2BDOTV5sTjmKBZhsqZZblcG4CA%2BjYTcmOBnFY4vJUZRqdU5wA7BYAmL3rNd125yOQ%2BuTGUOL%2FSaA%2Bsebb9RiwisUf32yd9jfUCqn3O%2Fv5BwC6CGGlit9UwJpklURUjQaT9dkGlyqBNv3bLBFXp7Dsmk6%2B3r3ImzNaiJvaZu4ki7DLj3k6xjCzF4EJH1FNunJToiPaInHOQoMdSXKXqx3689a%2BY40xODL8SrQU%2Frmj8rumpupWp%2B25sqwdCzaTx7zU8SVlwmHgZWcdqzQ57m8CeEC1cLpHNfwFfkPkjnUSLHT%2B3Y5acSCA7Y11NrH7uiQygemKjDx%2FMqtVzDOyNb3e4aC8ugM01DQlTCMEpoXC1FEUBmRc3PmSvVyDM5RtIoH2s7f7hWQjP%2BTM4YXopSk6fHoLxxtaMrr0iJrxviyybiQFJ6gQvxRX%2FlBrmqXnpAE%2FVmVISrqpmva8GzB837uZohqMg2Ijaz7GC1BPMMq0Wgki2bwx2wL8BlgjEDIaX1jjfz3HGROjASBGQbnV5%2FaPTKnnao2%2BW0Km%2FBE%2F0%2BkJPmiVUhfMOfsv8cGOqUBP0lp61GNEYyXcYl%2BYR2gw4OvptaqhZ75eAqlRiWFJQvbi10oF2OdOGc5fQekADl%2FAps495OSds3u1ZcjjTFxQFGdxvSCqm93iwoCeBFLBbB4aXf%2FF6UEBo%2FhL%2F9XzwSlsSB%2FCndHV0ZBJVv9y1SuVydOmq0taY6dGAHNSgvQIGQP%2FpLcVecJIY3lmYuIBgDdm9z%2BC53tKcxoiu2JYt5qq%2FrZBUib&X-Amz-Signature=ba542d44ca11bd9bb38969c0a59d1f82f7242b2cfe0ca2b92e564bf64d850fef&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

