---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QG76GBAX%2F20251117%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251117T210044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEOz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIAU1uZ0IaJEf2MfgKuSfYPIbr%2FE5%2BU2gb7GqHJtzIjAAAiEA6j%2B6IwQntTGjrOpycOB%2F8UEIN%2BUlpBPtUV8PrRiZB20qiAQItf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDMjmeAPw3MNm9m82circA9WESGs7i0AdEFdPB7nOVTP8CK%2FYhVxU9Cy1EpW9b3%2BAh4AmQKDmYFGoOSADt9u%2BgDmF9ib94%2BYJvhljoSC9GA%2F7ZrvA0KPFm8%2BXQbYWVXaws1B1RfAn0GuRBiO9jh%2BAU0UBuRF0MOu2EwKxZzZpKPnlJbw7LBMNpm0pjtCnkNKJMSwIAoG42YHF59RM9acn48UBBAduhBpT7INmCxGlQEWZ9TNLtlupBiaZSjM21LnQKIDFu6i%2B4xQLK41RaUt7AIW4pQZYyQsnJXlMudfc8hTP2aWu2LGOcAp0uE5oQg4rxfdxxqJxxOZ7IFg4yDAe%2FweoIRPgQvCRIvZpTsEZqpnvdp4KqxFD4X%2FELfFS8DthxhZLn9ywGGjezggLb6UAHHBxnQQyMYb0QTTCDeRJCCzyypK9R5YC3rO53blIa9DCGmdBk77aVy86O7bex3CTI69Ib1xHCthgeJ8%2FvM1pOFtkj3Z6mSlnVDKtl7yHVM2dyURTZRfU2Kih80JT8cCLY9adtIcujXIY9fMRKd8vEgJhDDb9bKOzNdlalw1ZAybORGZ3%2BPxuZgTi4ZbiimGJMoHzxaQrxa3UKw8oL7VK8aZYVvkMS0kbX4HT7j0hMnuL%2BKzfCQPaToMvnp62MKb%2B7cgGOqUBGZNLcPZjl%2B8hviQCQXksfu7f1nHdt8EM%2B2AjC%2BcxdJ7X7RwyvJLbnAYjRAsnWFJiak88gZw5%2F9XXRjOJdY9ojFy%2FqNvRXnv6zAbwbWCP85ohywRIoMxQ%2BNn0Xk9Cuw2HWbbBGwc7TFWaFUqZ%2FxfgusbRc9rTca9Y6%2BgqSDUmKNumZsLXI%2BcOJNrk6zqkNeOYMy0oNoUOWGYqSd5m2OCXw5o3%2FAUS&X-Amz-Signature=4527a60d81cbe4bef7e6ee638b78c7a3a997f9a0048ecbbba4b4901fd5eb6795&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

