---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WHTDOINK%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T110043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGMaCXVzLXdlc3QtMiJHMEUCIQC83WoyLZuTrMw%2BzdyX2o9J0vI%2Bi7nPPblGZZ8n1miSZgIgPnY6pwxoRynSCH%2FF4%2BBresUZk6i0mmKJfn7YtXw47VoqiAQI7P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLeEG7yjkfVWDm5R3yrcA5KwFAH83j8PYjh%2Fk53zKIfKfh2b65BVLQZqRI%2BEc77modtwZxU4Rv004bx3paaU0YuK7jskUo1HWd%2FjwxXS%2F07pk3i%2Bgc9asdvkzG44wlh220AON3UI8eRs64lXsakucv%2FtG7uPVozc3QGveWtJxEwPPuQHDJs7hYcer9SnE6xXL8bVR%2Bl523J%2B2ViTlxUNccwOc4Kly8756D9cQvgkwICdMjK2emUsN%2B2mc2%2BkeQ7Jyf8oHu48Ie3ruIFDXw5CVWZ6OF5oCf1WPDE%2FLbdKml9I6qmGeHGBstridf2Svw7rYOWcRBjrVabkHWuM%2BJe5mAPa5wy%2FGJo8DaxIMD6RXSGH8gdjSQaxBhHUKU1HqIrLmhqigY%2Fe9wnjd0e5y011bZRGaqYHmFKYfwz6EM%2Bm3Lc7Cx0RvXbMQVnzLQMsY2f6yT4Aosbg%2BWWC%2B%2BxasHVpVeWt5vVRzBP%2FdXhwyrLEASnrEYYl9j73mkp6SyHTu9WeP8ia8Eu0KzJhqwnoWYkmSw6QKlNerBbAqVZEh8xv60S9ESEpN6BUJ1kHSDndmUAx0J45Y4ISwIqx9kC%2B8gWkU8B8WMxxNRU1U0feJaetRx3RwnnjYZYhgbXyk7kDRmKw4dENZHFopMKUWQE%2BMPzp7sYGOqUBGCJUsKiuwyg%2FlHJ1P8WS1dRoQqgeQ3exrZXshXAYYCm0Kv3sOba09Pw0ln1rIqdCq6QExedsvnJ8JPmzdzliUEbYXYjCwooMsSgBNKcGnwH8qXL1WFg98UnnanQ7voS6RFbhwXLAF3dTFMJaWVZ5MGIOcRtV8i5GEL4kIh1IHL8BoYhjijxm%2FWU25F%2FcgN3%2BGXzJuKxAWhfDHx3VmFROlI5Dexji&X-Amz-Signature=9acb4824b2d1c0ba75fd9563b09af2b603d9cfdec7c8df9e3c8b465c5f00162d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

