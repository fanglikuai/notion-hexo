---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WTMVQHCD%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T060057Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDqLWT7hdRtmSckIphqcavrS7Pc8aoEylypduG2AW7MJgIgTSqYBVmNnk0emdd20yGN4T4X6NLEGAkcx4MuMBv1Zpwq%2FwMITxAAGgw2Mzc0MjMxODM4MDUiDLjhhSng0qfcib4tYyrcAwlqfSz4uvEHGEa0AwcCrJhNEBjuP%2FvAlFjZv5Zhm32k2mVtJmvcnvyRVJW8FbeQ%2BkwBNoi6dhtu6U%2FV010NReJ1cacpwEGIX9IJyuJCOfzReIYpg0wuAmZ5nqt65IItIih0%2FBGz8WP8wb0BY8sRfTFbO4Sh040N1w3X2P2%2F9n7%2F%2FyEwSbrLv6108CREaHMHrvoWQ6iU3LU7THkyjw%2B6xNBXlo0UZvMjQbk2czXTYVAI4Dd3gaO7NKAKeLxYl1uAxypDoShY1%2BuQgBMHcxAgKc0lAEKMU%2B6gGx%2BDONsgxt9jRw1LPdQLkk1hziZfIHGNvosCB0E7fjO7V0BZdc0NLxNC5VFNKW4NTf48X31BGPJpienNBF%2FXI%2FppFNiGAXyleofWMxcwjLFd2V0DV29qF68IGLGu30jZsQWYCZzWVcnMS8EMIzzoeFr7IgrBG%2BYqi9iWuSxLVuCJnggy7ashg4wSbStzwXghGcwCcUPTWibkEekcPR6LKuBlaVz7Gg7HL%2F9yiy4KKXiEA0d%2FM%2B9mNcaQAjGwSbYwvztnRp8JtWQxK6N8GNR6GYLAJpjucwHsEqALvdKMJTXcl659rRvwqdtfrK97494Ss4zo01DgkkRTPR4Y85VGtLwc8UVKMOXkj8kGOqUBl4BjCb8LKGp3%2Bp1v5PcREZ3%2B60gKtriR%2Bu4k5vzpQfjCt%2Bo4ioz1Fhiryo8VafMQ%2Fh9FA9XfadCQfq7wW09sd3pVsK68ICzS8vAH9xpwFT5f3xocQbRkh2IKfAS3Gw2sWMWGedq08myEG9Nh7SjjSDI3tebwm%2BHH9cBiOSvhHqwOqBEF84fp8hHRyxKu9EILubBCYHjRhh7UIOikPX8ZB4MRTbjr&X-Amz-Signature=15d97fd8cf965e082c7ead2553b331391fb8da7662bb59a80745b94708e4d560&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

