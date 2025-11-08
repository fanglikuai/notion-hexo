---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662GVASNLJ%2F20251108%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251108T070042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAcaCXVzLXdlc3QtMiJGMEQCIBCGp5%2Fh1yZgrG3791nGRIggONhS075DJj7st3Tuz36gAiBuhLJOcSaXgfcX9Tr9vFyDzict9HNXHq2%2FNSPKCulZ2yqIBAjQ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIM6gXdJbZr1kjseWlYKtwDxQSAOh2ZuHJgRbEYCXiSZB6Pm%2B0S1AevMrBtpLJvAkEXkRexuFbAFAdmhT9e3KG083RmrvS%2Fy33M1FgODJwcvuY20fP9bcew2V8ckvlBAgwGdMLLyZriD01K805ZKIOhoPP%2BjJTiD0G2ceoQh0%2BndKy0qUNi7gLOv%2BJxS%2Fl8cQvbxtg9RzuUgVCw1AbcKgvKwcykEzDM6k48W6hfC%2F%2FVfJKbdI5MqElbFuaVXKi0Thj4Thm9JjJwx0VYOPL5jLZwalIvL8o9LQqSvYlul1DEVAZZS%2BpDNA0QbygHmOd0xZI3f8fWPC3H1fmrT7UOi5l7LDt7UGQzcnX8g2bYJK5Do6qpkjuZaGwXVufFK9oWqf75kULRVB25Pwhmj9qX3SViHs8ft1jDoNdKMQHnAVeWeL%2FjEGfafbfc3mDBQZpwyVVYXwneRRIU4GSPy0S0iYAJxvCko1AoasacyHmX6vQfOOCNFP%2BHNWA2zbQA5LAfDbUnuGJEuXn7LtLfDoBai%2FDWn8UIRzrvKGGpx90HuPFTc0M2NAWSH4r1%2FwunaPPKd4tkWLRpvidFdT8Z%2BQhWpGuRDP43eMUG3MOBjISf1uzJe9J1347YlN5Wa5vIDVBD14g%2FCBqIZwLNUz8mLJ8w8sy7yAY6pgF%2FNEuxEXm%2FtMcPxQP3KL1rSPZx0SkR04MyAOeXE40fKL95t3v89x7TTDslNmpiWDFLynuFq4kPd0NjRvpFEbiEnHB2%2FKeSXvoVXfO2Rg5C9DwoJcBkQzCXgBCjgyUE6JuP6%2FmPrWwBqpxb%2F%2FWtacYqY0YVMNOeJCOR41kug7X8F1y00YigbjH%2F8C5xw33eeGo9%2B%2B1PQ2afp3IpRm2zCyzbE3usmqic&X-Amz-Signature=6d3454b5f0062c165fdb0e306ba170de8a1e024f8838aac9ed9b6c6047db0b24&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

