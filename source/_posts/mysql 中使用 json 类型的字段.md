---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667HKUNUME%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T060040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjED4aCXVzLXdlc3QtMiJHMEUCIQCbu3VtJJM6nqTK8xUSnz0N8LMAFYY3oXOHn2BNElIg%2FwIgNbiFuQ1z4%2FGMeSeA9WLGEASMA8hCMXPTSoWl%2F%2FXijQ0qiAQIt%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLVXt9uSM5Uon2ZlrCrcAyp16EMvOc9D6zQhIM%2Fot4YTDk2MLxhGqUqb5BXnpxno3xAp5RbUaLMN4MyQDC9DxNKyFu4MjGAks0BWwa68uC%2B2IrLGVSHcEKkq9lbL%2FnKoXCKHTdEco99k1NgKE2hXGuTdRfmgC1OUowX1Irrym6%2BLi4nkFu%2FQSPLtcu4vpeM17%2BGEDGOPg%2FYoGff4v2oaCpsABEecoxr%2B%2FIqy91%2FfKT0YSMZG7De6USg%2B3dkiJePfVf9XRSVtdIWls0ueLxkGU3Ud%2FS7rKbS7Fiv6YrfYmLskAhM8PpkcKUE5kQiMXvBwjavli8YQefz1Zg2JwF1Olx2LLlkXSaEcCicEdqHzm8x42AG98OuwYi3xLp9d%2Fh01y4kg6dnO4jK0aSOzE5yn4ANMF8aF5XjlighaEYn3B4lYG%2B%2FhK0dGj8CFi%2B1jdT6evvHcKReBX9M8EUBR5xYTb9nUzwBnT0%2B9JS8u1d6SaIjncQesLEPFbdOnklvKVifHRc5ra7YOOSTl6qXQ3rWrCdsU2hGBje4CceAFESU0RYcTxnhiVP3%2F8SOVUAtb%2F3RL7HOVR3jUOvndgvaw2scj4ucJwGEOtUMFcUdCQMr%2BhuhplGqEFbyXZBVaQZXXre5GZ6%2Bo4q7l08RFNi%2FUMLe2rsYGOqUBTfAMQV1mtCYB6bG%2F5x3HBdcr2d5i2E1jxT30eIwg%2FHsimMUf7%2F9hUhGDemuOnQgEgPHmD3oWj5L%2Fzqkd8KkdqAz1KHMd55AXXY3nWddeJb9iSp7WqNyOIOdWQoOt4ccvm2n4tFBDesTBkHsjaxCzoEQb%2BrFxb0loNSxcNR4nq%2BvF%2FuTQAWMnPJsEz%2BpNTLXLvFBuA8pRbKL3EAdl0f4qGDtwAvD9&X-Amz-Signature=b90b190aac0dfc9b29546ec5d68f4bf8f4521d990ed02028f9ede2c177d618f2&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

