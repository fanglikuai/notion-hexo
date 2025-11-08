---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GEDTNOM%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T050046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAUaCXVzLXdlc3QtMiJHMEUCIF8oXOhN1xTeLCv4JD%2BkRIfZzt8zlbJc8oWfFWW1%2BFY2AiEAvDakQ4dSz5viMTHe8rPMoSr%2B%2FoTVuAKh%2FBqChWfBpH8qiAQIzv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDM%2Bp%2F6TQsyJSGpA0BCrcA3xq%2B7e41aYxILYPCadbLblAc3tO5oZY5%2FrWkfKeOwHPd2qEMjfQ9mPf8qYvtK5iobmvN2mF3DdxHfSBBafQxKowg7vO82Cr57IOlLxN5nAMcz8kv8nuMUbRWE40mWaSBO6LgSKcj4JJN9RU1nbCohpid%2B%2BVK7UvGMyxOLxeK4M%2BiFQgFUPe8fBNudXwJtuXvmdPDhqzcjQ96UQbT7bTnKIv1ukGfkfpkNSfyCVNIx0uxrUX6zTDxvb70G%2BREVom3hzRulIy9NwEJRDTZBXL%2FUSGPSrgraTzOIngURwYZVxOU22K52HcZZZfJ15gsnYsOSNlP1YVgHeLxMQs8QyZLcDY%2Bgjar9aDIriVld576O5zWusidBywQKyQ%2BGtcMwUx%2BqMltnIJoIke5WUGUPOvTtlpKY1OfrF5n0aETAdNfh6cm%2Fvr8n7ti4JVj2JJ7VpfCrO4LoQHL8f6ONTb6h7I27O2b8IZvxCyIN3EOE%2Ft%2F7QANsvYWt1mPCLu5ktymQsqJaayQpxmyGpXGcqhYSfOJtAFyBo7bToi1RM%2FhmHCYyYWIqZWxVCpkSc7O8%2BajKMxFkqSPQxIylWpmKxoNuiZ2LVGCxZPmH2ltNkHRo%2B2CsKPUadrTDKToOb%2BYuSnMKKTu8gGOqUBDE3%2BMopCmghHtn88XSMAHnhPlR%2FD9MQFHiZrdhxGjR9B7zpn2BN9qloKIfRdEbHym1UvA5s%2BILib11przRUngyfsO1h8nk%2BgeJMg81qAY6nNYyGGLUAbpAVlo4ZOvyCTNPXwOkzQG5yov1ods%2BpPNy9MF8y1oE0y8ffscjMllZiCV0uPQrD%2F9T0xfQuT96Q7YYTf3gpfekhEY8wfzBbWY3gNiMPN&X-Amz-Signature=1f6a95064986164c64ab3261f4245bba9eaf4a8f5b63986c3d259f4a6ed279d9&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

