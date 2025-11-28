---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z7UTSFHL%2F20251128%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251128T130109Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIH0hw0MGMgA2a7q7%2FQlt3%2BgEPvSE66WfvzkVPEm7oNYfAiBDxWDnniASR8R7KqWXeL7c%2FCI4Zq1dMN7lE7A%2FLHQBaCqIBAiz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM0RjBdXM2j7OqfXV5KtwDGh8pZMXZC720APlhMtoW%2Bf5N%2FBoHEfEv6iwujV%2Btp8JHhbf1qDz0n95UiXZ7iVVNjuaQ4yyJTv0NtyXcrnqivuoVy3EWowRj1Nb5x%2FWRd64k8Rj9Lh0s%2FT6R3%2Fan1kOB%2F0tmaBnugzRYaGelaYzg4BgZpTt2%2FSiEOZOaU68bKtL9axtAOyjeCqfw%2BnTK2bmIdCnJ3GOu0prqyxKF64iAf0IUgGQbL47NGIdKztN8XFt%2FZlKFX8BTUQAARFL%2B9UULgTLr5mLMaPEUAAa1MfG2f0TUOVHQeqgVnXEoD1E9NuftwsExFyGk%2BLx7c9UYvvyILI2%2FImXEPa3izmqOY%2FhkGMzzApz6Xt%2FGcQkRwXpTMhdnXY3%2B0bZHbr2RKSIPG471uPcRr9BBWIq%2BUdoh2YnJjBYLWl849DJNNHfe5x1MZNDUufJmG38%2FijrVmhCoj%2FW%2BDuUleeh%2BjQ%2Fu6ASH9Ns%2Baa1aLAzesJ4DXMONEBuD9btZyYHy4dG5KpDMW9hQ8lGfA7fxEahoGmgwjCFb%2Bq1R6ZHEqJT1fIOPuhfmJu3ygoBmwQ1e16tsYzudVYX%2FEX3k3SCT0jA%2BZqP2bQo9Q1%2BAvBpC3nba3SegpQ%2BHfG7OcAYXtRhLt3jkEILQ4lAw0tilyQY6pgEHQjXRqKNeqL6w2ULhYJcDKQYrdwTwAYaqKrRZUS2IaBOwY8Y5yFZ94CHiTrIAny4elItvFEYo%2B74v%2BzEmc0THfvWZNfurUiXP8nB6miYpdgELgmGPtuGpyjcR8tXo9XDHF1ruN2WxU8OKkMES2P%2FqkR9jhKVZiWfBay%2BklLyNZKs1IzCVjuuyuxQCCnB9EcEjQ%2FjwFoyNA5CyXxtauteE2r5bCGue&X-Amz-Signature=2d1317a2921a448d0ee838e8db4196162036d9f6c8be9d0785aab619ed483aa0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

