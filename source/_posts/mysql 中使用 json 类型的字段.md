---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665FY7ESTW%2F20251020%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251020T090708Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED8aCXVzLXdlc3QtMiJHMEUCIQCGifxaSxWjXL%2FyU5AGT5I2wzCLFraG8EvbVBDCLl6cyQIgcCTxCrif9sW%2Bu6pGiixoEOA5ceF2XNvYnlX1vJzYE1EqiAQI6P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLeoMNueR%2FnrfVKGBSrcA0I6buwmVPklhy47dTqWJS7nNmVC9z8Z46jpaR0Co8YxjHXEt4D7PX4rPyjpWcWmZByDYRn2ALUqUl0g4O6yHepjtISZ%2FVuppXXR7lCyIaj%2Bu0s3vjWdWijZSlJv%2FabQUdNj%2BDPTgIEUYGwgU%2FZvWanicuW6nmtXjQf8S9G3Cfyb0gldbZgnYKFQakOJrzRBqyfGeWCVfjttUuRriGWUuIU6wLadlAOaGNyhJmQUQQ6DD0LhY2eB9g3xpg86dJ9ItQAdbMT2lHriT9yPgLm7DlcTOVlRjYkDZwfNnhnyeczNT16FMZ6iewiRIejeXJqDLvaNquiuYKJtRUPHfFQC4gumYlBYedJzHA4XhAy%2Fqb%2B3hM0PHnGt9jREUQa2NZCGOnSyTSwptO4EoC52ylbK7%2FNAh6Um7YZ444StZXbZmRUYFgeeshd1hcE%2Fpmp1kksmiD%2FwgHwkb7Hg8ZoJkUpkJHhuflmLZpTYyIRwCQeiU191gO72ZP8c61GbxXPAnslxqxyG91iFgquLII9WauSbHzJiZws51v4qckqu%2B%2FF1otT02gS%2BDZgzY1tAS2lrJpd3VWURbGPWWOZ5BH7uywVTakwai4IiuqYtCQMh9BQVrnHyXC6RCNEykAFWmAnRMJK218cGOqUBP0VODZWzoPmdIN%2B%2FVdrVSSamfVOCHyQgDuB9x68EZB5lM8KW3Sj3WbbW0PRbaEDWiRpQzPQ9wS%2F6FzZop81k2tqMVUcWmILsCIIwlqq5AuTabCFVLbW9sQY5QNFoYoYCgZYd4NbIv4j1BoRWde5SzpmwhIZcF5VQYQzq0DTvjHBxggjqwpP8iaqE1KvoZ2e02e2g60wpiU7POQ7g8rIgj%2BUWLg%2B4&X-Amz-Signature=b4a24db2d4cc02f9bfa4b928605aabe0f3adb1643be86825966763f1eb17e116&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

