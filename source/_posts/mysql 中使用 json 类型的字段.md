---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WK2RB5XT%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T030044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIFxL%2Fon6Si5Uguus8vCc6hzW4Z7ie%2FWC8pcbeUSvT%2BroAiBV6aVPnqi5Gpw7mlXfgH8UGLZs2mbOg5gtwo%2FgZKp6KCqIBAiE%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMZ5yIhs2L1CPAC4xYKtwDT6cMKpS%2Fxh%2FuzAv0uQIShMsmLDH3kRBMPOoNdlVjHZtKzWEQIiAwQulL5YZmlK629EjkuWR3%2BBNePoLtU5ucjaWtew2BwKXMV31hTpafj4NjQ0RiBkWlXJPONCzV%2F5tOhDaX7b4NRYxfH6K2hnfqVW%2BdsuCzrabsCkaPtgh8mWkeYbu77dQh2RQIo21Yuk%2BVUR7MQG%2FH0aIyBgohf50lSM%2FK1FhUfmPLgwOJ0RdrGLvzdC25LWb8IGspX1l2KCTwtcM7Xrq36phtrUVS0WjkxzDxs8mo0FymAUU6g%2FbVanU2CXxVYiu4JVClzuxZ6iK4GCMsNUSg6tcCR5u3u%2FuO1xaZXvegMY39t2QEpcQr8k6IJfWuIEtH%2F1wLV2mpbwVD%2BxUdIsKA2021y%2BOWKD31I1sjyp21TmAzByik%2BhsUYxfG1IHr7O9pZzzfikY09f80IONP17sM9GhUhgWlHdMEGTdxrlC9EeT2VylgtFij%2Bg5rJmArJZoeAshpAENOOROSomiTWJTcEHsHTFlfCFaGfABv7nhTUm6QySPIvfsF7DOk%2BikJLApM2Cr%2FXHq1YJEiPmIxv8n4AaEGt9rCesAEk8PkSoyo%2BCM1GuhMnpowFYMswcHS%2BIms%2Ftypd8IwjYDYxgY6pgFncrQP0f7rOw6RKMMwuGFA61iX%2B%2F5YaB1bICJ7FmFOmjBP3HohP%2FOAL%2FJ9VB5Jlamqg%2FvjwJGD5F23DZV%2F5WtfIKeGTYGgfiYX52AMcnhRx5TyX%2BNI8cSf03TH9IS4foiT1bYFy9HeO0eBCKuKRKk9EHdFZ5EbLdGZm0Iy887Rewv9xHt8bgESxgm7OiA7OkejC3sUy%2FyF5JgKxpOqfg1RxtpHRnbk&X-Amz-Signature=51129860ef84d9fd8ec8474b268c0f6cbc2c3350c766fe462129a70975a8af6a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

