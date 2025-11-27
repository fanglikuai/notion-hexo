---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QVDEQ3NP%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T000041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD5CDOGyI4Xuo4mNFhgGn7KDls8miihkH1aXYLGYF62LQIgB52qVgHmdD%2BN2z5U83ZCCTmLoaTSfjp%2FvOCh49P8Y6MqiAQIkP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNg4XObdxUkc1uqj9CrcA7aU28Nww0vLtq%2B7JsvYj%2BC1RTl8A4U8JqpP5j8%2FQUtrg8mOj5m2ZNH2TG86Rl9xtr%2BbDmxP2qGwQ%2Bm4CFR0LT%2B%2BhhUzdS5DVFuTj8peMR%2BuBqj9K65Km%2F1xCKLJf%2FO1o2fxmHqLv0F9ZMBwWehTKtsCErOeJojEUYNyxYraV3EmhFl7UUYVep6pzXBvGOUGyv4eom67J%2FcXYFbY4NuPqT2JBB5gMjDB0bHh6Axw4QleB2%2Bd24w4vuxowpISmgsBjvnylluD%2BIFBOqSA15S4SWcm2DauQy%2BKfcUXoyIGSoi0LzxWYqS2qYxJJeBy76YOTyr%2Fn%2F7rWDeP4C8WIDXqESU5utGGDSNFQ1RkYbckabjwFnDADYwfQJK7YXkAV5%2Fb%2BjWLaC5F1xUR1Zl4jnDwllOzw2oZ6Y09MJyUnHiscNtuAvGta%2FTFaBNZWA1OFLow%2F51JM9e0wtcH%2F5iSARiMZftvw9PZkPf1sz6ecfbKB88iTUHvBQ9fMyF1Ojt25qd2rQOO7UvB23AC%2BlHOCS4af2mHCx5BbwnP%2B%2FcDKIRsD8cBsHz1IqKxqBffYlmGxB5L4wsEyuIglaS%2FafnIUyaD0lQvx3MT6gX1JB08vVRK60mM78BDrJO6Y1vD7gxuMOuVnskGOqUBqs9emzfPbYbNGYFwXg8cNl4rUd4plzmfeQBD9HidGTVmuV18TZb3e2VHtOCHs0Cm%2FBbJXH3YPRGy0%2ByN5CIoBsVW5KlHWqpmLFo9JASdOhrRV7wtl6RZTLHFPv%2FjrjpoTdNhcjrDvv3mv47WEnjmoWq7ZNM5elV6QlTefFnKFeg7kUbn0nFPPSvY3ktC3dZsnoXE5C0uBiXMCCQ84P1w7HMkiGAZ&X-Amz-Signature=8c8aeb584b257002f25887c9d18e461d93bd89441c0b76d8a71b573d2b73d82a&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

