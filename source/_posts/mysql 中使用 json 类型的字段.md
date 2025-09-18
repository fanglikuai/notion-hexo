---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TAGJTY2W%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T230039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEE8aCXVzLXdlc3QtMiJIMEYCIQDJhoKLWYG2ltvaIsrgoQt4bftJaIWWnaJkPd018bYB%2FgIhAOCQ%2FjQqONTY2d65x%2FI3c%2BA1kYWJO8hm%2BJB9D6lqsJI3KogECMj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgwEb%2Fg%2Fk%2FAQX6Qg0T0q3AOcW5IdlGl%2F3fOOkqVeROG6xXmT3R5YJn4jdf95gvJhf3s0CLMTSM5x98bJ2UELQYp%2BtcGYb8mHjDC9F1zD5EN9l1pBfXrhx9hcFw5OX25OrpuBdwToe9YtaaWNBozAM32ZxwKpkpXMjHK42Gtq55YPyeTkML0q%2FPniRqCuK8XFfiUwIXqMPxn7uR7ia7kYhc0gzsUzChXcBAke7VF0MVlI5W55cFqRT0w1dWdY%2Bq%2FlzXj2HC2%2BU2DJ9%2FmzcB9auvD0HpyQko6E5Zu8ccb%2Bucayp5yz4kFH2EkTFRbUFSyVZlf7acfOeCXFGwDRFX6x5ztw6FbDD7mxRjY1GfhCjEqhaHfuScZ5dO86soOt5g0LGPBmaymcqzgbMETc2V03MfDzJQROf2nW0Vhycw7Y5OOdTVP8il%2Bf7mT%2B2zvhb5lD7QbN0nNdIBjtjmOd0nivXij2t%2Bvg87l%2Fl0lIXpPv2divK3S6kkDzZez9p1z6%2BtIGg6uRYwXRUhtCb8cE%2F4EKQ%2FkREqr6uWtx6zdGyNu6nbnjDOz9xqM5mTBEVBMX8tJ3eOx%2FO70J5oJSe%2B9PxJTy6nEVnSHz0RGZ%2BJUFj2OosYyFIgecvVph6gniyuswtJkVnZyHeT0sn%2FO91%2F84eDCgnLLGBjqkAWlxToYSnNJXO2oF8DTEy9aWcowillFYQ4eaxJTribWGwPSfG4nGLlxVigTaHOIg512aQezng0Z9GXRfslM%2FwnUHHsCRnp74DtTDj4sfwitW3UdlGB2pzfk3cETJnK2EuOVAHmeQk4hbQw70MispW0gQkF4hyA3Dw7AQBpDYOTueYo98F9bzJm0GQZi7xOi5WAUgdeuQCfLSnjE0YfsEsfOdrip%2F&X-Amz-Signature=166cdc74e4fe09f53014323d53ebe4e583cb1436a81e6d2b56389e2eaac25e71&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

