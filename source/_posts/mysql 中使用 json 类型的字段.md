---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QIZFTQE7%2F20251026%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251026T040045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD4SMhQECTyllu%2BJfJ1lmS6lRka9JWea8kIOnP7QsKskQIgQNzTw8WfHYB6k65GgsS1kvrLKYZNK%2F%2F5ZPv7PVvaeGQqiAQIgv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDFcHS9pCeFEo8tMRwircAx%2F46xMZp11WfmblAB7pS1nQM32XQ%2BJeJmWymtZn%2FX10PaHG8pcysr4IcW0Vnlqnngiu%2FIapIZO6b4Vd02PixGzzCl9kdcIIIVIlXtosAy9OMcX8EfivUmQmT%2F%2BuQ8C0sR1ckQMqaP2PR0XVT4ZTTh6JMG5Nze2XC6P3fj9eHKZR86qFuWCNzuu6uB%2Fhl7DN8nPdFGzcDkpiMYVFQ3MqdN1Qs47baysLbKE6vCK3jPCtLaSHsODjLniKZ5fOPhMF3PIzY4UBvFoZvwGCeINhsH%2BfJtcmn2l2kAtkMAsDI%2FvGbGDbcCGq5ru4P5SyS1WrM%2B2vdSIs%2BBqJQALaLCbxlviltpkyNsrSmcVtlwpppoIxvOze%2Fx3HtZmPO0Nb%2B%2FTwPID9uIf9SxLCUWZQvpOos3DD52aORiQgwILQnJtwMUwvsao9peK8Ng94wVoHCDBIl9fnE%2Fz9glPQasI4f%2BEsOsfpTkdSVlGz5nBKNboZd9j8oicvGAoUlRtrYLdSW2RwtoPQZSmmzq0yfis1A4vb%2BdGcuUgJpsHT0VXkVOpsKseoT%2FXlU9CY1rA%2BCY4Q66WYtpfweAknbRBKOPF02%2FB7ChgwXeHa0YlJSQwW2f6RNak5L8dQCTiDmILrfYKFMPDv9ccGOqUBGEJZ4TG0I0llbJmcj3tLyTGnFqPqIsvI3r6yV5Eov5Q%2BX7wlsib4cwt4Bn1HqHpuY20Kru5SydzcXFpsoiOCjD9sXfT7EXnaG6O47AcuHdS9iNA9P2Fuh2vOsS%2BOMT4TaMq6QSNmg2jGln2p%2BKXsmxzoMTK3CpFl4wlGYfq5V7r336BZDP3jB%2F0AQpjWNSsoLX0KDivi9P%2BOpPa50lbKDtRU3ZAw&X-Amz-Signature=0d0ab4c7ae834ff6634f75b007e53ccb7fbf13aec3412798bbf34279290eb22e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

