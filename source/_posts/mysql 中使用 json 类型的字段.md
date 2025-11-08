---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466ZK3KKCKI%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T100046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAkaCXVzLXdlc3QtMiJHMEUCIQDhogvDb9c4JZpvfC%2BO%2BxcjYldhnV5EJ%2BoD%2FL7H5otzxgIgTWPHazrHH9oGKluUzUVXVl7JERubBg8JW4pbDr9ZeiUqiAQI0v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFlK5dF3bnE2OGbEOCrcAykwmBQSRvUXHm%2FVlY2c7%2BxJTOeQu06XeWiNRV1x2q1PwYv7ba4CeuZK%2FBPt1Kn%2BaIczz3x4twnE%2FMSl%2Byb4wSlXDfqoW%2FnG9b4udDwXdf5L4qD90Dw05IltuG2xcFSwzjJRNWnL6ZAsRznt%2Bgj82PDuZhtj%2Fby8lVuDa7VJR%2FbpTp%2F3rODmBv6H6WYCuChU1UGiu3RCzAdIKYVrpgLDnS9LRWvhX0JgQb1KUYOwtUMCDRy7p0Yo6152B6V9FEWAPzdTAmmGrcZbNu5BDxnoAGXF%2F7joW7iB9uPtx%2BaUJ0NBsvbUVMiNpNetUwnFuMwMklakOllhOq3dtUCwaExeXCGtmzzlOMzFHxNaUo8L%2Ffiu0c%2FgTvWPkrE6OMyefRIKGjKQZAFxVeqPg0xrYcUHGL91MbUOn16S1KSmn36%2B1rNa6CzvhIskoYo7aG%2F%2FUjp6%2Bnu5P4a7%2FQACoMEj%2BUoVoFr7LHWDiZ2FcLYcGNWPI5COr%2BixF2O9Ol1334lzl%2FQaai0vI48qSFgHoYyHcx1JJF%2BglgjnANzwlmBQp0lrxIT8XZ2Fdv%2BAzqX%2BRKlq9IMC1k7CEh4rA2GdnudtVrDvc2LCgy1KCy8islXedulj%2FYIFbPHkKYm3ViMUvvzlMKmOvMgGOqUB1eY8LwqojtUuBlf6vgFoZvPU6n8Sukb6cl4491dZU6qCICxgvEyAemzkCuow6vlC%2FJBagEVkgy7TSKdGRKJ1QgiLjqi5rKxZ7S24RZXRP21uubsWq39Kz6Bjum%2BbP8FKR7yS0YsAffmqEJa6uyak8Ni2iyDKtbVJD05gOJs6k9orfj3QhJEXV8xDisEgmXOgZEcV%2BJ799qzRv4Ar7RuNCEzYxQ02&X-Amz-Signature=bd0283f720ce16a34ab1a39d9e275848ac9f677577c57b167595e0c304b9ebd7&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

