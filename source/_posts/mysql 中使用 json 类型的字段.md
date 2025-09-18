---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TF7VKP7R%2F20250918%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250918T220044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEwaCXVzLXdlc3QtMiJHMEUCIHKjEIQJOzru%2BQYKd%2BAkEoMWa9gqnlyUgZofdsszFBUYAiEA0noZ%2FzQScdM1qPKITCB5lOWEBnMF%2B3Rd5mbNGXfRB6IqiAQIxf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDGOFT11sRWpfZJ%2BlESrcA%2Blw%2FX0NGJMnWVjPYKVsMWl8gImSJ9X7EV90EF2WYnDtpdxDrz5tI28iAawVP4BraJYV%2BKmtM7lK%2Fc9yxbIovtebMjakkb5iFmk36CyPEsnn3j4I%2By%2Fu0zTafZPFtzoZa0RXASZu%2FvPwZdk7TjTJNCBjxY57rT46WuUh2EwQPzrirAQprjQKYpxSFzXF2fE%2FubYP4l6%2F2PG5yAOYSalZeuhsStj378IpyTX2MnHIrQKjnh%2Ba5qIdeqSNr8GRkX7R%2B9bP7v6bRL0p8BiuYQABfvHowhP1uHh3u32NNNSNNinZjW1X0ZoJiGV%2FLrMEKgll7VOznM3O%2FuzcFY7orSZeM7nCCKSC4wwROHO%2FQuVcPzVu3UQOjwcNVQaw%2FrNeFeZotzodw1HFlpSMV9Ty4WiQ7uMxOlhu6IEnfr0078EV1MZx4qflfGj9eUgiKtJhbKDKNRhCsEo41%2FDfncF5MyyxutMJewttWUagZ6jUUSkuhBCWbAjGG1QCDxy00HoBwsseDl2zAxLTMR%2FHMXDE%2F%2FWzMi8%2BWiPMWsaYqjlil8Z4jrqdpl6kMh%2Bz6FrFtTF6L0NjUsIi%2F2673HTf3LZuU%2FoNqx6HTdsutHbZyyWaxF148rp3breg62RkPbV7gdprMMbIscYGOqUBfqcGKwUlF5G0eUey58X%2FxE6pMOvdwiz7b2fe8giz4QEHfd2M4g%2FMDFyRVsfSuXcMm2QazkYmMDWqDtlCWVdLqcIEE5tAv%2Bk1MFEU8npAixJerLI4IGgKN%2B6EL6jBtWMRUF2Eh3%2F9Hy1auiFXkHv2Rqzal02QOnH2wgnd8wEkZJczJa1I7E7DD3qzD4uG%2BXDIF8cwdHm%2BWfiDR5vLR%2FnlybP7Taq%2F&X-Amz-Signature=ac8762cb5137d99a9b0c2afa178d6cbe77cb3916c82b9b399e48ed99de90640d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

