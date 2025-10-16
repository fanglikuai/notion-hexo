---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663W6UN4MR%2F20251016%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251016T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEN7%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCkwkmukkU3Axafh2b8HPMdj%2Fmx80TC841vSS8BkJ5izwIhAJoqRii7HbUsydNE%2FansFMoJl41TY0E3XXTyuJRTThZLKogECIf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgxDGbZ4D9ISt3KqFWcq3ANB8HZOckrUmrxicAOqJNhjSdiMIeMwghK5oTOjmZKrWCqIXhp8h9ffKBlUgnAOL5730aIJxVSd1IP1mXvrL8sEKIXl1jog3Kj2N4se8vtvzvZA4WrVyg1TrYy%2FPoOGjUJUU%2FyaNxkO2E8Q2sQIGBFQWOjZNnjN7rhTXiK8C4SBU8%2BlXHUxZfOTHFtGaPCEkUM%2BcFwPK8zWsZ4iG1JQmIEW549QbpIDAP96v%2FXI45kxnrk9uC2O37BbiR81p7Cmz5Ks3tHFNEbpG2BZFRrD97cOun0qEm8rARx%2Fk2tuXGlTGHGqnlm2U8KArMv1VWYZ7IO%2BS%2F2NRUux9cYGcf%2FzLAP9zy9TpQzIXyVYX%2FxKnHZVasFp37X7u2gGJMNt7gSNsbCr10opXsDNArMkzfEJLBM%2FgbKff8nJsPCMG525uR4fgNM1KVlMA7Gd7GwVlzjcFxRemleMX7F8LDQVopLEsv6Ene2laXpfH%2F1zVBsE99jodSWccTTnRoHciXrrkcujlYhjcIowTYxXeRe1litU6%2FzS%2BwL5A38Zj3%2FaynH266LIwR4l6x8WmLVyczU8dOuIYhRo7stdiGGWGuCAobTwVTX7eRl2KI508BeWQ0vvfJ2faUyfwNRCOxNj0mg8zjCGjsLHBjqkAdVJgOtt9K5T0wigEmQu9av7u2mvorGNkrscig6UcnQymNa5Qgacz%2BxW7svVq%2FwcYZ%2F6uCJtMwRiLRoR3YD3KddI4mptBBE%2BWSdq5RcbmHzqLrFwlXxv4LbMiIHottsIrb7eaLjzvZrqW0MCUNapDr1lxBrzvb5waYcpNkw%2B5a2Z84yBjeBlzvvZNaKQr0Rps4d6dJ5dLAVdDzAd195oR%2BPLYlf1&X-Amz-Signature=a445c213a77ecf19d0b7052534edd1052e4a35f88323d138a8ab6b32fe444125&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

