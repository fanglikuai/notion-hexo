---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VZ4XZDQB%2F20250917%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250917T090045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJGMEQCIBmjsgzq0TILy6yvDRRqZW4Th7FBpEQhW%2Bv%2BUWW3MfW6AiB8pV48kvKT6hW5OYFVYMwqaklW8PxXQ9licKeFOxMYlCqIBAii%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMBwJsHiIuFQ6EM8EcKtwDIHAScLL6kJzkT8fmKZtmOEPGS%2B7abNAk%2B1IkJxC16qCFudfvIbzMvgQrPyFDRN8WrkXr9bxFe2W9rElE8gsoEeInHaM1oO0cvpRQOemKtf9wzigc0RMiBqAiF6SvxB25Nuuvc3t%2B3DUIwmklt3BZGe48GaEV11R9%2Fg4P7dDEJQp%2Brt0NGwZDPpp8ihNNfhIW5tsbJ6jZQwfG9%2Bj88%2FO4BuCsH6lqVyq3UbE2Ebh%2B4A9P0Jl4iK%2BUIo2tkQm%2BF1H3wLxxTewcg1koUBYG8t%2F8W1mcJqfCu3L5RN2HvOciBpXOkTNBR%2BWpp%2FPW5vlonw5257ohLC68ldkrfmWIpMlCA%2FN23VxEDzye34t4A8VXV6Mxyc%2FQSnRXX9L4kWsbnRajHDZMy%2BuNpgwM4kFhUW9thvKWfmAQ7Sq6619tAaEO9P%2FkrYH6IpqfuMgO7Q5qGywdGWNOd6E1EKioLngBKFZefHNQ%2BiI%2BmMaa%2BdLjiPx4yvTrteNWw1F7MbIjzVeYNUjg3E%2BY14V1Nw38y6bjLYG3Rl%2B7eFZTp8Ke5mvNRNjTI15P7WDMPyd7271LwkjAuNmKLnjAtK%2Fis6WlxD8ZE1ebE17Te%2BzBvY2LZM%2FvHhNTGqiu%2Fcv0Ur7qk2TQV00w6%2BipxgY6pgFLO0JfSRoxd6HxaoAusMIy9aVo2%2BiBxjLohuxQwSLJPOxuyinQTioZZ4HNIbI7OhZtZ%2BUoZ%2FgPQ83asAL85EN2mrkABwmkocLNGTbyt%2FDUEQonE35HPwpHUeO5NGm95NEA5eOPYAR5foweneLUWJBqqzDWGlHQnQkA7Tc84LyIv%2BPnrxevTnYcMxCJaxSJorqI9VZ49U%2FBNJ4HdF1CFzDj11%2FWqL1q&X-Amz-Signature=35bbe94e8ab1801c27fdf1bb6233d642564057ac23acdd953ef814523dd24b23&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

