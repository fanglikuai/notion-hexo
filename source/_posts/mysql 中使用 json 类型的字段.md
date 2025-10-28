---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466W5VXZDQR%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T220037Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAwaCXVzLXdlc3QtMiJHMEUCIQCfpsiUA5ZAj3gcbo2lO9cyQVfT0BUimo4nRl1TkAH4rAIgFc5QOKACv5aQDLbDVE22FVtlK7AcXUjI0Dtal91%2FKuwqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCVGf8Z78q2OUTHzmCrcAzm9IMkoJY%2BkstwnW9muJ%2Bia8gpnRCVBzdGEsHtjoLVXSU0HunF2PUK5Ex8kEDAzwq5xnib7f2iMsw5doQkTstl%2BpueCjJMzGreVWJGEGRDBjSO5sn6WA5yVLFGj1SWNsPOajFoX1sHeOrvzEmMm9AXMQZpeTCt9tJfW78A8stowJMk2PS3f9Nx8qXDG0SXbm7y0uge0kMCbBOglyxXIeWxVGDe71tUV5UfCF44paWLS9Ia9kEjgdT4%2FfKCdrJeKkZb8lBWtlin2v9Wxjn%2BTGiouHyO1VQTbPWaimY%2Frd2DJ6sokwuTrWEtcMD%2FPdVusL2V97V2BFJDVNbhqRwGpsPPVo0rkW8U%2BPFLTVB%2FZ5ErfRLvc3oSatM0PGk3gstpHU8pwod9qXdTIYmc3nnkUoHD5UFzcS2iEYIyjiGTqC%2FA95CtBlmXhW75Wk4YG52m%2FzvrU8UgBXWsVU7LxEkDJA2AmJUeLyE%2Fk1x3nROQ2gpWLrNn1HgtzQAvKzVuYwJg2AGcD%2B9CqNpXq8d9W%2FnuS0TVG%2B2p3diP8BZrNz3pxaOQl8rhnLGpTTbdvehzqqZcd%2B3vSfDHLLFzcdS8qhpeAY3nRIi4t4y61A%2Ba2VpVK2QZTemXkKm7qV6vWIjnWMI66hMgGOqUBPLkS7aiJGp7rRGzljnVXzJCk%2BBpgZRZ%2Fmay2EyJP3i9QCg5Ej%2BFL8GEIVGJu3db3sjRn2%2FSGa7nOhCTIR02JMbd2HoYcu54l%2BG5cN%2Bn0ATAhS0l5xRccxToBGEvox6y2YI3%2BojHEUt76kuLGxyU24cJK%2BnlB0AcD4Pqs8sRlOFIR5TnhaGGQZegewJUU6itIujsYLQXZV8JYqZFakxcaNnwPWBIe&X-Amz-Signature=ae074a6bffcfebef2e4063faaa2a9946364e589e9b58cbe71e6980636d6df5ae&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

