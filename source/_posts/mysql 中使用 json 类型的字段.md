---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W7F6J5M7%2F20251121%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251121T110046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEIaCXVzLXdlc3QtMiJHMEUCIQCsi6MSmeeFLde0xIA6JvtPBG7PNI6K8r3f14kzsutEHwIgRejugzFGrfvTBWkb5tBwJ%2BDky%2FyBf1mwNIddIUutCMwq%2FwMICxAAGgw2Mzc0MjMxODM4MDUiDMkPi7zLzf4Mx0b4KCrcAxPry%2FAf27DtWHk6KuuErOyVOHPDqnRU7dAWT1mjgIg11NehaBkp0bqb6jwfYmZUvwDwEb8gvME4Kz1BTXiqK0ISQbys%2FjsYiNyrY4qExZ5Wri4kY%2F5W6hnyvpSCesy9msYhPSv6100xM17HJI3rAvVXr8SkaH%2BzrFLUR%2F76TdDII8M5zzMHk1hMkEEiP1WsCN%2BESftagTQHzg0LLmbjsk%2Fs2WhaGZiOTgXrgdSEPgUJWmSptcOkbJ77X6JF140WgyrszQ6Y%2BCZiPSv8xr36ojvLAAycQxZWoGuHY6jzUZy%2BLTAXZHNCsWKdJOtgwpx5dEFIXXT6Rxdwt68BfWrhw%2BmEIeOIpEa0kbRFAbJx%2Fyl%2F0uY2aM2Bw2I04tf4fOEFTPZOt0CkALofL%2B6OFnw0O%2ByjFIOvFAhChI0Ra%2Ft32G%2FhtuwUCmlWoKN32vDEoPb600uo16gRUnbs3wIGi3gQYSgBCRwEaWaNe2eF2dz19l27hfjwa%2BmAW04dFkwbNM9s01NizQH%2Ff7q9ulBA9JKTNi18z7DYzXpWlWctLz7mzmawbqe4AO5XIOIuEs5cz3VE5aja4UgMq1wRmb8E9XDvhbsibkCrIt1JzYFxwUY1zFZvb9LIh4Jbzd8mE3iwMJj3gMkGOqUBPwAYPxtXsgyMGuQtWNnr4KxMxfpXmxCCry4yw80Xhrq5GOxohjTStT6YJS%2B8ePLvJw5vI7BBD0WrfuOjK0qf6%2FQSuhK8U7iV%2FWetmdLNJPT2WmYiO0ZR11lEw%2FvhXheI1M14nmKirNJ0so7MH5kgx3qCUc%2FMy6ZgtKmE9sMCu9bnbyjPS1OxEJwP1EyBJqZ0sub3d5Sndw1oH1z3ef8EnVrkZMtp&X-Amz-Signature=768aafd4d60e9d9803f0faf631771500e46211f69d372dc063e29acfe29a1daa&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

