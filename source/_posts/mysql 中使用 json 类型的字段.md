---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663V4WOIOH%2F20250921%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250921T010046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDhgQAoiua6WGU3Y%2FN%2F1DMsB2O4fM9mfWzZ50ZZ29EQ%2BAIgNsogi0%2BfGjpWE1R1WAQzP3l%2F3xu7QyjjRRfnhzXwEBgqiAQI%2Bf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPxqFbei8KYrNhG6YCrcA3FAGtNDQRWFUsGkBW3XSTz5zKSwhZ14mghMKhCcwcLfx9YOBzf49poHIqwtDXvZqVCKWR%2Bkkia5IZBUIH%2FjghR7onJi%2BSmqVU7zrow9f257R3Mj5TyTwyms2wQlovu3iO3HNUN49LluYSVim8AIr1QQov68maxWcEAA6%2FD5Z0UBeLHYwra17%2FcAbjEx02lCF1qbVUl2co2qvqIJkcJPJZsk0Ki5x7DyUpUrU7NqpvHNfTkpFkeVB8seTn6Ds88TukW4pZLM0y%2BF0ZPfMDma%2BvUiI4WN4bkhqtYiteKOSGcnEhyMexRWZwHmC26JeL8Ohol4fQrOLWHrMWGL6XB03O2aBHAFUeeW0N9C6EnFi8WuYs83FdIe517XHVBpuxU1LnVCdIlbMcJI%2F%2BDCKKptOqnv34yoo4dv3CCz0bPaVvLSfYZn01By2ldE1PdnE2djim2DzIGKrmrnd3zD5KjP2wKp15Ip6gqaoCR4PpYRuJmnJeLVdOkXKvWYr70jTLaneYGYhRFl9vrJnUpNfivYpKm42ZsneLCrePT%2F8Q%2B3OBWjepoEziKs6qSbOe6I%2BP0FnP5%2BXsC3X4%2B9yadRUIfJOOYaG5bUCKK793TDR7lf1u6osTpSrUYLLGScEMmqMJSLvcYGOqUB3a0hvWwx4N5helFzXZNsMoQ8zBZnc71B7zjsNUTEJI%2FJhP40DxqTls5OYx45I94laDIAi5XIZvBGh6XH7IkYUraqyV497JuXplQ6r%2BcCr%2FVeiB%2FU4PauXQzowRtaebgTHAaLYQyrj6vK%2BLuXPf6D5LV5ZKZ8nuA3kxbCoImRsaBlVG5M8zOgvvxsnR%2FK%2FM8htuCQ%2FZ2L7hQ5N%2FgmOrNfHJaSDMiA&X-Amz-Signature=fd155f617a29c86de4980447b0f4ca5fe864f59610e16b77bbf927b6aeb427f4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

