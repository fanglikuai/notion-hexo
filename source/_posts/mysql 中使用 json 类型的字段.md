---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466Y6ARI7DC%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T120051Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGQaCXVzLXdlc3QtMiJHMEUCIQCu5XCb1RKbT7VZ%2FAfOqOg45UwG%2FKmFUkT4Ff%2Ba%2B24EtgIgHe2ksVJsvA%2Fd4hUMs86%2Bwd09UyYMAOQONtRpKXIMcekqiAQI7f%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDPOP0%2FKlRGDSBgfKKCrcAyOPyceIXMRNMXbT8TayO18K9gSW3iuLHmpFRW46Q%2BtfxQjNxt%2B%2FrCvi6hrQnf1KL2SR6iXaLrQmzO8bTUdFZ7suUOpZy8ObDtx6C87JLNORvWi%2BIBi9Kak9tAG85DFacxcUwCZZ4O%2FXC7CRl6iVt1ZgZpqq%2B1WRIsuXb4eUAbsLRQkeUsomykRDKsr27ZBpwiUJDgn76R0LSAuU8nW2v4HaScUSNApBN2sZoHYpBp7qpxrxFXXkKhQbUoc12pS7BkfIF%2F6OjjqxwQY4EFDAqpEzaWDr0Y2qIlS%2BkKYqGAs9XuoSlQlr0bL0csiHCoRGHfsvgqyNQLpsWcLXL5bfxAN11efOCva6zsNgQ1lZmsBEKW0DACsuJLbyA9JAKb3J7ONhupfosTC8Ar00qKp3zIIkLh7jLxeDABQ%2B%2FF5WOygjBpd614rMQ0q7zaUuYL0QEwZkwyNWNf5Q2NrTDg21PssWafXqGKsvZ3KOhn%2BhjeQg8%2FJSJzGEFUX1oO73Pr9MMZqEL%2Fxzgj0iICaXnihRTb%2BtqtG%2Bvl2xlBd3Pr%2F4wTlwSursIHCveInvu6ndT%2B3SNAbxZ4IO66nqQAeZoUB7sLP%2FJ9ukldFwBI5vQNf%2BrG18RS7wmM%2Bu6cCAk7m7MIOL78YGOqUB%2BFtxqCG6C%2BZ4tYrxmKThdkHXHKtiUCcW%2F%2FEuSxSi1jy%2FMv%2BCJOS9QC8jmmEwi06ok40zU51pLFKCwR%2BnG51U0jcNHPVonaVXvEW3gm04WzcAv8nsiufXqCN44NCdmH60cChQOOQAu%2FV%2FgrJzTxzc83TU6wF6vAzLS0IFn6mG%2FzM6%2FP4vPXFKAFRuU15JK7pQnKv%2BVmorRQc93UePOFhR2IYsVUd7&X-Amz-Signature=ec18195628b1c772bd8de6e3ad9cef3008540fc5235940eead5c0a61eeb0ff20&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

