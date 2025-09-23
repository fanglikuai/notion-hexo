---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663CQTZTIQ%2F20250923%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250923T010052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEK%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDk2aTBSTAmECutTU4Cw%2BhJ5ITOoKILCgrZovIK6lC7SAiEAtg6tWSly8HfKCTPqqs%2BJag6K7tQx0f8joteUQcBGbhkq%2FwMIOBAAGgw2Mzc0MjMxODM4MDUiDJsXHjZ%2B2Zyq6u20IyrcAy3VV8RMR0J7f2gRZjAwHaKjt%2BwNu7IZRwtEUZt3Nwqi5ElFLf48kA5aMIr1UV79oynh053lhryFOMHPrAjkSTkAX4hOm0bPJ6wOQJjRM0aE6BmdNn4L6qNSIz%2BMdlkjwuqJ3iq2jnThXzW1DBptvJGLuXZsTXhPqUBD9sh1nllrcJ30D9YOJ6aZZCSxWJL0uU6L7ifooLERxyrlR7wor44dyg1DOqSbXzFnmnCgDbuAPtHabhhB%2Bw7wndhXB7bTXco8NHlYJ1x8maRXgOn4rE4o6kzFH9Q94URbeb%2F%2BBSljlHhirfQ9%2FiVe4jBtS9PLs5%2F3gJBFYJwYJ%2Bdken1nJ%2BcPDkPD%2F7JilMoL1YzWKOsvKRCejUD1NV70wbziuuEZaBUaC%2FFJ2XgSVS7P9OHgE0U2FF6OJw3QAh%2Baxj5lWvskr38kG33yP3z4shBXZE7eI9x9lb6neJAHD6%2F9L8I6rG6uqtxEGeTLxsTB60E1y1VCyhpUuzMjEySj0tWuSiHQQn%2FJ6CgvWjQGDIqfS9E4bVqtjyeK72CAR8yTLvy16D4vC%2FzrxlXrZm8D6KewOsuVK%2BylhfiwmxwlJTL9hKyPOneu5Nw8iyvNlGJuqEtqQIz9cirNDRvdKTmLIaJZMOCtx8YGOqUBIz5fEcnlSzBUR8GeHVeeQlqL0vKadIWJZMnTkuY2M5%2FGxTHPn1TFttqrqZT0%2BqoJ2zZFRTr1ZTYgruBvwSOPrRtGqykbwRIMTrctU5pXEDMeCqxmLZrt6SxnulqzKPjUcRQjSCaM8OUqrdg5TE0SVRXShj0DO9%2F%2B476jx5bzXgxA0ynPJV0wObAgrylg89NgIHt%2BLkHicvDTLba7A0pmWPXIQbgu&X-Amz-Signature=97126e6eca4555ef9f1034b22db3255ccf1b419261f5f3efeba2067196754ec2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

