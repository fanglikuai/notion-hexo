---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SZM3CLCK%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T140118Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED0aCXVzLXdlc3QtMiJIMEYCIQDGipmxiw7Osvs%2F2GUzmQcYllV8RhKe6wlw2PaH2CFtGgIhAJIZVDMqyEQANFjeWv1yf4w32S9ssFZEt%2FsfNEK1lrKqKogECNb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igz8em0mz65Bjnr3QDcq3APYJrepqERYaVUAuFwKtkXfMHGbrJMxT6e5ADaQbdjE%2F6bijmWgnbQ1hVrVdKcL4lxr1AM4u3dlXJ3JJgCoU814pnUPX0D%2BlE8LVmvK5jaTnPEs6msdWV4PNlxXj%2FtxxmDOjcjXj8pmxQX7e2jp5CXHqTCa5PASNnJRzA9Ap9VDekYa3F8x4gdrHrUO5alhebZ%2BpD%2BiNzkahbKtk6u2Jnlc9IyD%2F%2BQIm%2BOxTMhM2lLQ5%2BTB397PVgAV4eLg3wKzzJjlJJwlIiajRpNR2Klk%2FypegVBB5n8uc1JuwjdgZZYjUqPBvVszpt6oE7Lr3oT81lUDP0dFo27Lv%2B5rCSL34ol9ouuNF7ISsbBuVOS3wmCTg80syfLQAgtHhMiywtKJhPyBckcRWKHqoAj%2BbKvEPBVw4I5dAq9qvsSGAH%2BC%2F0woG5HeCphi0pL%2F5T2xokFBzNHBTrVLnaNaWCxkTbDGTtaIffLedenrAZ3tBv0%2BfC1tmYHXHGfMeHBRJv4qwPwx%2FfrSvp1PXQNa9eyvr0tlblv5BZicv6Q7LNKixBdMRhY2hHFzUlfcUW1mTT4u%2FvD48lr48eKxpntaT8VOaXqT92x1lcuZkWjd1aVwXDCqJjk2rS9x5vJyxFBy8zDP%2BzDL6Z7HBjqkAT1mf8taAoSI5kZ%2B7CIgE9YMqUBmJ5oO8ZmNTgd5IkyC4lzvCCNg%2Ffmm%2F76hDaLMLrp3VOdlcR5PvtCF8aWO9XpElALxrSuUVrcOnkT4L%2FCHnjbLmd%2B9M2O6DNPQ%2BgNpcgEseFnM6MqRv68W8jojIBOBOcAx85R0py99yBV8%2Fd0rY3K73Q1ezg%2FcxuLKKCpY0ObZNoL%2Fy4Jo7NY8x9fxSDZjK2QR&X-Amz-Signature=6a65af72001059f5acccca12a0b01545d6f0d132ce3186654c9c55381a35a66f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

