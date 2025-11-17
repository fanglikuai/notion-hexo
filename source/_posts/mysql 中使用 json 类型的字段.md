---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663KVPKOUF%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T130049Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOX%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDIpOz%2FAgf3PEaj7mExwaPwInGkAC5nhJ6KyYhkrFY2XQIgNo6c%2BMiAkP4bqQJOavTGVtDV3GdJJtWRNZa0NA3gwTMqiAQIrv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOT0zDhMszvUhjaJxircAxCfXmy1EfHthQARuKEjtnit9V0j1vXUHjnucP790M3DRgmGbbBF7u6AG9bqDk9rZ1uBWdgxMQLhdk%2BUYmESFGY4zkgy72RIdz1P989Gpzs9pNr40BpGFPZeQXbLXUvQhb5SWWLdqIMdd2%2FatvkwrO4JqrRZA4rrxcXSWpD4ru1vR4s8XZfDo%2BY%2Fp1U7RVoHdY42edU3xHWl1INz86Jcw%2B9bDG%2BJifhJUDF%2F6v4zFggwohA4bYxTeTVjFkB%2FUBh0GUvjLhzdvgPYMLF9G6y3VkCXrTTNiUxIfTvP4R%2Bz%2FjhSF%2FBN%2FzsnM0Nbkv0XcXitM4bTnouvMMMzNQFXqsp0Ja1eETprbExWaIGy5DDRyYB9s08yy3klGDWYHD%2Bqz6LEdMdQYLe%2FqO8853Zpi2AfA9K7Dq%2FCxnOG27ph7zrSvJSX2BKLvA0Ak0w%2BgzCOOYVl8Yebu1rDXaUrCPO4S6keNWIEy0CUbfmY2au0aWcoOSvjyQXAEzSI2kNa%2BKtwczm3MyJrEZq5cqITjTvRAxvWJQ4Kr%2BNZcG%2FMhAyU26pI0UWSyQxn3QfoebSyFM5mRrF8SI5mXYmWByoxbHDnPqVJyQCQacwz51mJxeFQ3A8txjhkRi%2FoL8hRH8n3JaQEMKmu7MgGOqUBuuNBmAVI4C2YaPO4F5JdYuay6VbuvaRfa6KyRavCNk0zzIGCNR6X8qg3xilWJG61THBNVuk2PG5oXAscEgRqnxB4QdMyqAcaY36GA%2BsIMB57DFDIFT1lvEo4QfxvSfHKA%2B0j0J54hPVWbmL8WUy40xSGZv%2Bi5%2FTqsXrtAln5kgWFmeTKw57aUvv1kACG8j7GMCG5hpbmPCK73QqNLO9FzuzQCYrI&X-Amz-Signature=5ef30cfc0bbaab81b898e36a691ae2117fed51b5aafe8187e22273cafd2a0950&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

