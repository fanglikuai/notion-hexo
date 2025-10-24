---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667D7TVIRN%2F20251024%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251024T000052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDzohNPtDtlEfEdPdDl6E%2BVFwJzj9RLSzn0XlYymvRDggIhAMYtBT8TuIpNw2lW%2BJBcuAQXQyGnnlljVy5PbZclw4NKKv8DCFAQABoMNjM3NDIzMTgzODA1IgzYgxBtpHPOLltm8d0q3AN5OAc%2FhmUSjwxJypT7mWyIl%2FxAZcr%2FmGiadVPEq0TfzZwm%2B%2Fb1Dmx5KkaLnOIWs5CbywsdKr5R1pQpQN1L9dvIZLeeW5FMYmxcqsiIEMKyeZ49Z3Q%2Fgk%2Fv3e4e934tRVjBx%2FcSCt%2FomKpO4351n8WrvRc%2BPo28SK3ulHao4Ke4Cvo4KO7WDn41jfqtNGcVLKk9crk0DSnlhEp5Cyd29yRIpUzTG25%2BxtOJrU7hV2%2FHluqEHt9MBzimiZ6f%2FKwdER%2FSGO1QDqIp6iCZ%2FymMeBgyHeuY%2FIfbAHHVkrgZCzYG3CZd97dzVve7103e4SE%2FcS5cJY7EB693FdiB8mImicOuBtkZMXhoYpdauwmrOPe9V%2FR8dZ%2Fx0%2FruZSRB3YKqWzhmemFNUJk38LNCnT5uRjKzpRp6TSzS1f7%2BNr3Ts0Hs4Bb0rOP3lnhs21TE3qeQhNArgNX2h4FUvvfWdY2PlVdvwsuoWoDJ1E4lMp6ZB5qVYv3TPz03VXb9588%2Bkn4Th4eW2%2F9GDgiXLbosU7vceKOPwnDbRy%2FtIBCQTtdJUO3v7Y6ikcOU43eOk8dnfifdoK0NnktzckDpt0GQ6Ztqz3nZoZh8v85yPxy1ZZ%2BDd3tKmVwrf57I0UUI07zZxTCN6erHBjqkAW8cOuyoSk8I8k0BqSN4GmBYXxGiP7KmSZZmNVbktNwWL4WpFm%2FTOHmnzHAVO4A4AcehAc8tz29oVmWj0QHbobRnk%2BDCfYYm%2BKpXick1EfSWkTXGOGO8%2By1nOT5%2BtU3ttwp1NYdJ%2BWDzzaLcSRugR8gK0rg3K0Su%2Fu7dPxY9x8OzoegtuYY4YVeQh%2BL%2F07T4KI1TGbFpjXOBdYA9%2FYSIcptSdNL0&X-Amz-Signature=4fd7f0341be4a1ba26f99b47dc804a2eb0a22a0a79c4b225752945eab4ff4f2f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

