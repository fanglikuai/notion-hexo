---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YAYPT2ZV%2F20250920%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250920T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHIaCXVzLXdlc3QtMiJHMEUCIQDhy3AVM%2BoxdNfJNGqCZc1M2ogqiABo0xEFlIXN2Z4hXQIgPxmlmA9l0uND2sGw2JxWFx7TSZcWI9P24xjoPvaeJskqiAQI6%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPbLDmnAIEfgJZ3%2FQSrcA1eU5CFIIkus44CwpvlcEicSlii5UjPiwwxGbW9BvyT1msuk4EUfrPOf%2FW9tEiI87UT%2FQ8j8oGAa2RU6zrsJCSfs9GwtllgmYiMwqBfNtYtM7EpAFy1WVAtb0fjQoNzgNNttx0j8MRS7zC8hCTqnle9U%2FWDX6ti6u9STRw1GZn6Z9ndOeAW3gG1xrlMn1JP85ErIwdC38lH4NrbuJZ7xSx59PTGbtbip4VQcl%2FXoF%2B565Bc3%2Bqf68Rb9y8MPcByyc6zIMBxtmxQOiTUXRsZwum9qdn%2FGhX1VAootr%2FzxU%2FNfxEoTqpQKlEi4xuxtlonUaOLY12BkdZT8cBMykBxA3LiYA%2BJE1NIqV0QhBew0dYWgqy92SbzeqPqzF6g71KqTTkPBbEnbb%2BjWmiVy%2FIsnJdlYK5WjXF6ZVGqo13crmrXsT0nvtiUT1zmZ9iHYH2Gz20x8c%2BJttXABEJh%2BVyuYSVckcwrEj8zl7TDskSKHJhhlObkP5%2BVc77jE65dLfxEft4rQfm%2BwsmnE7Mum%2B6FZy%2Bp%2FtRyreuRWVy3%2Bb18ACWfmFoFnZGXpCx8SxYnB9Y63K4OBSeNNCDC8kIxllwS8HLBeVH6M36zXGvjcmbuG4mmhU%2FINTp2px6iqREcTMK%2FqucYGOqUBCCB%2BqfuCywSGIHu22sH4FEf2JPoEAf0oRyNReigASJh5jyYYoFB2q0SERuyJNnIk4T9zKGjj3ycT46E3mY6Q6ehiKJjl1cTxycebFxYHshioqneozlC3cHNSvr2%2Bnh6q6YOeDreHSpQ2zuXAleA%2F1M%2B6lsjgWcHomJHsS6FntUj848n14FK0RgB0g30zUwDLwqSZMPKdTxUXwg6F9nM6VsW15x7U&X-Amz-Signature=5f7c983c2d1fe4332671acaac5418235e1aaa948cb408db20cb7a0a915e19832&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

