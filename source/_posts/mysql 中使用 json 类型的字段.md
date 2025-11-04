---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RYA4SIFJ%2F20251104%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251104T070051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIHSL1s4Qb4rfZYdo%2By8OGD4RLHTbgplWeUfgPHbfSRn3AiEA4iMhESZtATnerxn9g%2BxmX32PLrZpL2iDZ14ggRsnYNMq%2FwMIbxAAGgw2Mzc0MjMxODM4MDUiDMTxTAycHYuuGIMqfSrcA%2BGI3axjBtLABn8OW6YaOmiG3GrClg%2BTpnsJroYrD860qWjJCIF0iqrZN4OobofO%2B5hewoONelLYwh%2BI9ZWaL8dVay%2BDdEhSp7G%2BR5UGap5%2FwLYvfcBq%2FAgMZC%2B7H1mXdm7NddzEwI16sKqHGlUihfF1606Z8raKUdOsAQUFN28lJG2lQuYH9vGVpa8rpCj2VraaA86TIK33rkYwkTUXgOjebqQR3O233lkvMcIp1c1cIIQcvJguSqrSGWbSwqZmsGzDb80aOTlHsuQjbANYEDbcy8j9DaccnNgvEStKfRglnH3sGZfGHTczL63hLjWkqkYNWkfYwJZwnSyiGNpJd9it5qkGdFoWaS7tMDZicwverbluOFZfmhNP6xaVVm0AZevAt1EVNHMU%2F4BocD%2B3qc4h2AaAuHheCwnF1fqkZwJpuJetxPYyAsUXhrzFSlOrXd%2FgoGA2ipnAq7BuyK0ybIYcFWSfVfJ2ChvrZQabbiax1kT5z9A6Nd0AVGNdPyX6gWPFlSyE6eAYTCDIDcXV4zPan8vlLAAzMoKVi4l%2B%2FosnJNjXjj9UTs940uqWwMlTTAtEEMN6wR9kUhoPrm6QNHNhyASe9%2FhupyW56dXRx72zUGNt15tV7x8pu0DqMOiqpsgGOqUB8Jvf8N3kwYrbuVeHMsvVwLoescPBQDplK%2BCsGrdXVUcFXoHGToAE%2BgjeyEw%2BU4KUX9sKDOezW%2BDJSW9HyKNcM05rn0%2B2LQ3vQkRa8KeRJ1wOl1vYMizDdEd9vsB9QX6Hht6iqiFhxhajmeJpgQI9ocxH41rOSY6s8E6jREStYEedt0hShjHUtEm5Mz3aOkcFC1j6SFetaTsNO1QAlmFYiWs%2FYOQL&X-Amz-Signature=66d8cb11b63c5df896b1b27dc07174a60d6beb4394d360828667e2718638c7a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

