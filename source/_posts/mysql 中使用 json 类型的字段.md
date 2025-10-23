---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VGD4WAPD%2F20251023%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251023T040048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIT%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIEMqgnRzSeFT4DZmm4Ws6yoCZgB6u%2BUNA%2BOyeZAsscxLAiAk%2BbTrb2fF7YrJcXZWaJfHa8dqy42%2BMq1QD6bfWfUqiCr%2FAwg9EAAaDDYzNzQyMzE4MzgwNSIMDsL7EAh3PzNE6AqtKtwDvE4Z0DzcbgDTirmN%2BcyNvmvZn8D478hpv3p5%2BYqkBa6V69T9v6jyk7Cl9nrz9QC2NoNit%2FFDdnoKxbKcPCLIa99UnkPZr6ymFlz3zxR4g7bPgLkKliUkvG5JjgJm%2F5U%2BqHiinqSa2SV7E2BJ2hvCDYF9JB0FMC3pr41PXZM%2FuHkr3gE0ub0KXr7iFyTTHNehEuFRgQamJzHCmb6%2FjuQxYEdnxVYJI6Ce4J%2BjyV%2FjrDuV3MpaLHOxdbXbpsqT46rIEc7jTrhhPifbVYsKIwjsgq2BtqZBEmUSBNr%2Bt18L9p5MTmUpqmaOOydttNIS49U6A7pOowEiMsioGCIoVDBxlXGOEzotpb4ZuQy6epFwLJ%2FU9%2FuUw1on4gOywmgmubZfzagNibMQE0LLATjcDaqDQ4ccSgVCmRVBgkjT9Pu%2B70A3qG6554yHWTsnxVqqldSn9csfFUU8pBs6jVWvmiT%2B1uRvS9F%2BsUJsUOySUSBbq77Ff5ZOheekcLm%2FjQXoSN09asDgSEfmf9FXhjLMo3zyTStkPSGU8z0gDim83pRdFGqsavc%2B1VtHMha1AgM%2BmuE1GT4N1Hrr0rE9hISUR%2BLA%2Fs4kyzaQ6JOQaF4QFqZ7h6poWpp6RQwsTJSzy2Qw%2BcDmxwY6pgErirBbN9xjfm0Pbcn8QkWj%2BF1%2BLvSCj4CkCq%2F4xCEr9acRCEoMw3MuWDfszp1mlpZEKkwWd5K28fLHSRNeEmHXJufzJkYvtGBA1v%2F1cu83hMQSw8l596Jd%2Fuojr2kOHHmC%2FekzjjvTin2wwA5xFbxsjfKjwhfVCtd1cdBP%2BGsOhCfUGvcqzL%2Bvi5ewUm0HMhIPLOO%2Fv%2Fi%2FWV%2F371wU%2FZJP0DYTf5n1&X-Amz-Signature=74f22d8b21f0292ae8a8f89aad18113a13c07ab28c426a5b69a26f3b7feeba7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

