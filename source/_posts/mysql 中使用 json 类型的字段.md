---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YNQB3OHD%2F20251120%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251120T220040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDUaCXVzLXdlc3QtMiJIMEYCIQCBvwA82F90mxb7%2FyJc9xMPqfARID44gKaTVU3NXglnjgIhAKAgQcocSp7qzS9EA1hyuKcMTKgldeBAvidanmXARSTtKogECP7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwyuy73zWn9uampfncq3AM4MT940c%2FsmOz9mnh%2FSI2%2FtFAKXrEGZmrcXPlr5l6ThOQ9Q3gviF02UzWmBKnZQbwfTDOMo5ukHRej9K9hUbLtXqmYCoNozgXZPPzQQ8mOeLrGuZ95zUMHm%2F3MOCN9toKQkxIpozKpAH9ZUGB5gprE1eFxL1mZuQhCdk%2BXSCD%2FsSMQZItSM6LDsEXLLdGL%2BMaadorkwtVZbwQLOjIe7lXG0TAOm7EkboE6xQSI%2BQ3T2MRys0FD1qBrvo79oGgcHyqh6Aa%2B3XBCGNuXSBisW%2BeyInGUu9kKAYAopBXKI5TWMlZsgUOKH3UoPURIVDmpR53KTlFT1oE7M1iDuu%2FsWFFYOn45Hir3vCHgAqqUblJEe%2BHWLt%2BYeK81VKj4xpx9sEzkuhIk%2FKd8Z8GAsfPacu5fKj%2BJePCmFTU%2FoH8YDmzIwjJAC36e8r3DttY0xS6yvi2tj8J4blyQLVC%2Bt1pckz9jiz0GaRauQRuwrCTon7poHwXu4X9uyot6Ei1hcEbiYCGjS6kRvALv4dtqz97a5v5n7hZcOQU6I3aLyvqoUmyu6w361HoXdbP50hqEke0ZsAbtUweanwXb3qTrxBV0xkbpUi0sLWNuVaINBAU%2BF2i2PEsQivzdjpn2YduSOTCy%2F%2F3IBjqkAQo4fXuVnYfOHl1Pov7ZFQSAeXrDsz5Zy%2FklW45oF21NuSGSVUgpatR8Yh6Qt92XwylqfB5msTP3v5CZkDDfLXcDeoFl77eT4Y6JMwhew6HXtx39rW1vvGqnTDp8TN%2Fh5dmQ7IXt5%2FvYgo%2Fi%2BhIGpjaA%2BF3t3Bf%2B5lKQgNkGJc4HeEmkfdZxgQI17lo%2FAhnueH1b0CTqEtF90y%2F9US%2FcCvIBSRl5&X-Amz-Signature=86b9217996ac251cd8230c709cc2fad2e0d0ce022e868c3852149ecde4213937&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

