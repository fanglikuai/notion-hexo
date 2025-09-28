---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TCPPJV4U%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T160052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDe8WK0N5Vt3G46RJgYQyhtmoiUL6lrcHEwI1GjHeBTdQIgXtP536Q2aykl6ZRQ5mPKVnQa%2F4%2BCjZNT2M8pCoZD5JQqiAQIv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDJAj0lpkT01KnEn8byrcA22eAAs%2BBwAzkx1iP2fVJT52QUFXcgqd2p%2BXCMUCivUHflLyMmu28JZXRejA1wC0E8rzpKqe7Q3gvTWGCEmQMhzUtssCWAPRb49UJYRi8fxc9Pj1iKlS37gm2Lhis7vR0qZ4kpchiN0Rm6QPaDLWbywh55mTskgYi7pMUkzjjetAhai%2BRjXa7bs%2FmZZRusJtPmLfkhDZog5obJd1p8wEpzXZEQcduf6TOzElo%2BsE2L7A1HgJVjjA9GtVb51zWmGZQg%2B5owDZ93Bjceo8bqI59sQGsPNgZzko2P1SI9Hixf%2FCVhGyJw5sbulnsucWc6oWshXg2gMziIDDT1741DHIFZTVy4SIruM6tPQmOspiXrd4B6HrjBgkKrSLaP5zI3MBKkO3qI3lssgv0Gq3MF6TzPxc7t3WIWjP1nQufJu1yTOb1yAFRKmQDjctzmEptTWzHwo6fQIlTNFtUUi590ALN%2BqbbpG68tkUa4SQl36o9ZWEGBISfp5u3gJ53ygm%2FpHsQhX448eDrhh23uWpF2udnwziiXSesDtpEmi18w03ycuiVUs3NiUwoeAISvCE8cs2unuc2XPOMj1IjLh7gfgB0U6tn9hZObH6BLkNmEm26iQT3h%2BViqHQn4NE7hEpMNHv5MYGOqUBjLUxgKOZAVS%2BdorlxUVrQUx46hOZVLH08ZbX1LCoR4KBjYsgzmFwJGoAUMsSLwYIOBlAexvAgg3fCPkUNtOtBj9iZfANc1MYVvSxmGBhlNcfRT2nAQA1r6jB%2FG3Zm18BHubsFCdDQaxqMO2sotUvYUKzYn5%2B4K%2FWMgdUJfOlfc8RXmxe15%2Bdlp2Xw1rIyy4XmIKACkgp3YxXzkdl%2FKGbUbG3voOW&X-Amz-Signature=14f735d4773ecb9036c57c6547cc5172e395749c130b839f57f095c30c9d43eb&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

