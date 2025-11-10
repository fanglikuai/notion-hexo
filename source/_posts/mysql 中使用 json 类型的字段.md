---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YP6KTGNK%2F20251110%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251110T060051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDYaCXVzLXdlc3QtMiJHMEUCIQDiLY66IWlisOWeT4QbbJHingk3eoJr1EZBfbxLzip9wQIgLZ0UnHo%2BDvdi3EChGS86Oj%2FnhvTwUTW5hUJeCBHThpYqiAQI%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDDut71d9KhxvhTKsRCrcA9Ty5ZHJ5AyOIpk3k8tjezu3OZCaY1KQqc9NgwJCeJmlGZm9Q7qFc%2Fje8zzY%2FNqvzSBvxdNjAmrLQYmQjHa7SxXZUqrX3oVzEO9JMF%2FqFBxqGRrybhGBALSukexQxq%2FCTZGiBhqLFoHaFxIiWXLJHjZdBORF7vd%2FHmg68341C2ol%2FGHttsaH7X9nbUZHGl5CBEhZ%2Bi1bxtA2sNwoKPDzbIoJzlNdDLzDZMxPuH6NzHjxmT1EfMs4bzrC5DZAnW1v8jVsahWjjcyflArBWJHLVWch89TuU%2BGXH4vN6UDp0WWKWeSQJhIA%2BL8zxS4Pm4yUGslukhlH1JCOaoeWLpIGopWN1zilU%2FU67K6xEveLRR0GHZ6U8rD4pIPw2lqnkBExJvaImBQtdMCMkhCWj01g4noKroWZOE7MHRrmJpE9xUGoD2acWgIzv3%2BifTtg1yfSEEWvx%2Fsqe7EcFCUusPJuD5xW5WB7RMcxZxbk5et%2F59uwNYNPUYF70Q4TpBuCJIOnGIBdO0MfjyE9Gb9QHxNHBFcPHri6ODLBJ%2BKVxN2mwl07zUWhp12NmV8ef41%2FM4MYb0%2FZdNhcchynsUmY%2Ff8b9dS0fSxi%2FvssNjEsuFWxVzH1ISovBKOgxbIbQGdFMPX2xcgGOqUB88TT5CgcIu4sUGa9cu1YBL68udJ%2FNzLzTuthJoztgMjvE4GqrB%2FnuDXbOMxaaI3xwXQkDbif354v1VdTFrgT2n1JF4zFKNiMDpeYzFBiICPpr5OE3eI3yGYX3tMxpvqCwTQl0uWMfuop%2B%2FbnUN7lTqvb0iUPZO0wGcSQ0N7z9A3cSOPCNGkP07OLrmUpwMokUpWWMZWQan8ge5mjD8aZn1OWdNJ2&X-Amz-Signature=855e5239388d63d05ad5fdd3ac4679588321d71e93f25e80670dd0b77135fdc1&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

