---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Z4F2WGDS%2F20251011%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251011T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEHYaCXVzLXdlc3QtMiJHMEUCIQDyeawybrtxFv9Xrrga1yUc6yZbF7TTmqf6ITFQO35IIwIgYKqhEwtETAsAgTjvaGEPPOWaZ4OwPsut%2BCjvVLOVc10q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDMBoGnA7e9La0MiR3ircAzi5VvL%2FShtM3j%2BOXg7VM%2Fr32WDXy0LXFkjIE3WWg0KrleqJ0cZ23YIyFjTIhomn3ZtMDS5eZv%2FvmKC9l0734Ynx%2FOBWmbp5pDYuLxFk0idbIiRYlK8QZMJjUf87XQHsYJEF24YntWtetlW8N1J16QVkfHaRUNpIOXP9YWxQgJgwXf24UVtWhaU9qvMFS2Fc53y9O0JmL6lyzKrD1x8fsRxRzLUJgxqd8zTdv9HPiplGmUkv3rfr3g7XtOsK8Dm%2BjHtApQLgHVw8vrySL8cka1QCeUv5SJfM%2Bmn2bODJT2CS7EgAFJuj%2FOm5sj6OSMxHQOcxuEFhnlKErLCuz3dbVHgCn1b1ujoZrnnqhIFN%2FIlFFjmKC2RAZmyOJuCbEVAva4Jsx9v0NcBpoaLw%2BPzKGUvO%2BRTY7mW2CoU2jfZMUTfMVTTfa%2F52vWm9IlGrLPegUt4Nupzn2%2BoG2WaYE9QvmVdjy0X5vvujXiQqAAE0he3A%2BXznyi94HGH7UAZigN0kYVk%2Bpt0s30XdJtnMtOeVYLdkeZvIBgdpmVE1vr2jpmZUY0dm%2FBUEi3181ZxnkMzdCvdQI5EUa%2BpiJ72X6GtLen8zLXSf8T79U1xymi3VlcyCBVkLhQHxWKepng3lMMamq8cGOqUBY00%2FDUSuLp9NYaw4Sms2Zt3Nqf41uazEenOmNkHg4s1NTFaUW1lBDTeV1PQrObWMjInED0g1JONQR9v%2FoEQeM7DvFexdB9GvbE43OVBo1p5Vc34i%2F2k1aOPTY3x4z0VY7UtDzwCPxyuTw9hw5RM5V6yOapK5ITz9UjKvscnevwYDWSQjvvdIFkBe%2BArWl3Uveiy%2ButYS998umjcwGr9hrsc6zEmZ&X-Amz-Signature=04d8da35a20bbe0d0ef67476785e5576db79fa694b6901b28dda8775e3715e7c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

