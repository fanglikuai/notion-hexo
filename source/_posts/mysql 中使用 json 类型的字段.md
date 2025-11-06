---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PLQZFZ6%2F20251106%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251106T190039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkpprREjZgcT%2BIMWFKaHTw2HZ70cBRMmpXO7ykwQ%2BJfgIhAO5NVTcIxkJqwimjfF695Vlgsr0Ucg6u0LuI%2Fad%2FxBa3KogECKv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxAWB0Gc%2Bs61VUCR%2BYq3ANIGLqp%2Bf0bMKwRzUlxZHIa9tPx4jpDVCb%2FXeoS7W0r39HBKX2VxtV9Ahffd2n31MBzMkcOd6zU1%2FKAn4KSBApqUQ%2FXJhzDBBqxJoSXI925sQ0GFRQ1wqOJVrHeVNSEdCCa9qeJNg5oDsNfHOH%2BZqt8mASXlLkjitJSPlfPjIpL8MyD7494xMQx1d7xEfM3bj0sBudVI%2B6%2BEfiafhp79N%2ByChMlC6Gpn1tDggyN8Dd1h3KelWkhR6x36dCaRuAINkq8OBRoYfDZLhWvrDCqn%2B9PrRjBISufLU%2BagmeNsBNjsvTqzDzlJdjSkGOvrtZ3Pj9vYlIiau1Gxw5ZjwdTbAsyU%2FEs%2FxRwHYcUkwVWh1y359BL53Sfr2jrxiYV0SabFELIcBaE1g00zIxk7VsHEdoPUs3X%2B0cNAyWvPo9M1UK46telXz0qu1KJZc1OUzfP0FGOCYdNNdbXiSn8nmPgOM6OZrro%2B%2FThZzH%2BFPLDSVPSpKFZvwi3H8d3ywkR4eFzdqjBdOWn3g%2FNwGTPNcHXTxVv5klX4WiSuLD%2FOIyKEUoAKlkh8Ozo5L31MFr4dFQv30qHIT%2BbcsdyftbWfrKSA8gq0mtP%2FEBx3UXMxlGBAkbrjNSLaGpra0ofPCjgGjCpw7PIBjqkASD7F1c54Xze3ja9xoSsRduTkEHYklrM36oi%2Bw277sSWo%2Far2qmnOi9myxoHOs6Wubo6gncQbr00XoEe9u9qSqeuJ4li5lhw%2FnjYvNvVNiiu3OF%2Bcs%2F3bMm3CHkUI6GE7nEUpqEeAh9P1ybnVUY6kfojXPe9Tc2sR549rIRl5v7dMaLgaEZr%2FZZkgtJbJFI%2FXHRFYVCU5lA%2BxlvgR7J2uaQkaTXX&X-Amz-Signature=154e45f09ef02fe068d32d0af3fd5476831eebae8c85876edffe39adc23e75da&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

