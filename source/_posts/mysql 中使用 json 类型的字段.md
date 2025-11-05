---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VLUYGJA4%2F20251105%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251105T180043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQCEeQicvjGu1PA5cFu67gSydZ1r4otQIxh33FWJsaiDgwIhAOLANn2p4DEVMs1DqGqjm2zFZlhFi%2BbQH5SJN3pG4fNnKogECJP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igx25yWEf5JMIIRltgYq3AOQsHMQHYYVJCcB%2FBBrGs0tAX3vAbz39mvYKJ7YLaQ9B9P%2F7jE96PpqNzpdCCIVqp%2B%2B3LfH8TTk9Z%2FQ4DsUv%2BPUhb%2BVAXeCB3lt%2B9T%2BYemRp2H0hVk2ZrGHgnWml0NeBRbcH3hr93DF01ZNQ94JvCAGOsoqzkUszvWWc1notsxovUNhTQ93EE9lxroK1GywJOR7OT2OR6GrAR8nvXP1t9Mir4Du5PpeotvU6H8IXNPKXircrrwKJt919D35NR0hLy%2F0UGGOaqaimLjiDU1mUrZJrNTEPOd2DgEk5mjLLgFod2dHaDrqp6edpc4yixZAml8NOfgP6MmYfQYrXYcf7gQIL5ljUpMYO%2Fh9DeSmaH1R8fGaZT6u5nRbJxMsMO3VJJLPfke7WgWRm%2FDqwamWZBho%2B0RSbJ7wYNNnPD0jgTyIb5So%2FfyjzcFtGW%2FOx0%2FjwpsSgBwWqGd%2BLjBxZ%2BfDG8j8Iu3h31SgptqhOB1wpNMLKCbwvFnoUtp76ZyGm8U1HhUIi%2FgT83oer5fAiD7WQ8q6eF4Sn2ojUUpjSGFRNWjhTXyEQM%2BCAEbGkhqsTRLESHx08D0%2Fm5YmtE%2BfT0FWLhRNjJX%2FQaqsoGwSAfk%2FezyYrvT4LSbq5%2FM5wMOT5jDGm67IBjqkARgkQWKzbmtg8VMK%2FUIS%2FIcOCN1VhEZOcvQZc585TLOPGf7SBbGzy40kJ1b7Pzh58rEb%2F%2B41JFMqt3McYjQvJX%2FPve9E0JXA7h%2F%2BNRREgzdzGfwxhsVoQRNbqhl%2BeZrCC3ghhmqd4zTjUZ5lqcKoohZUnk1H6EOcGo5LvoJN7dgC2t40JVhxF6%2BB2K5z9VFJEBV45dkOWqwYibbcrxDD7UIxqCgT&X-Amz-Signature=7ea93140900a40971577cdd6aadfc334d5dffd4bc593fe047b8b99270ef61776&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

