---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4667FJWHFPJ%2F20251004%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251004T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEML%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIA1AUIeHK9oeLVpJFgKVHgeXbpGbqYOMmdhMg9y5FviFAiB%2FnUowcuue1ctK5DGDYpuuoukjy0QtzEPmXhKWRyVAeCr%2FAwhbEAAaDDYzNzQyMzE4MzgwNSIMcV4dPrwz8tl%2F%2BGLgKtwDJZ5A0DVwwW4RycvCHAKxiIir%2Bhm9uTx6a6GPNnzaa7qXELmcf%2FulEiE8fqc8fFzmfokT%2F93xqZhqag77XyDm4B3iydxk7RZTrza%2FKaEy23tA2ZN5zqVAdlatOtxPFY%2FZzFck53OFebXRRdXlA3hg%2FOSsKhmGsjn81qejK2viunN1fFd2eMXHq6DOVnoH77DvsMmfOHLBLT%2BRsb6jjHFINSNw0Nu3RFIQTGf3l9r0b4ll6chhBFvW5y9Ai0imszoic%2FjWEHnxr3L2kIdudbHkphZI1HCwwQWWplzHAn7tZrn34r9MKCTt818%2B%2B8E8FHXuW0gERAGbZITr02TQcfc6yxZCPuQ4QVJflQeItWxjBpHEbRfWnhUHxnYERCefG9caR8g%2FmO7i0o7LmEm103CcsBTCKVok0wswPKNRSEbB7jDbhpeQ3O3mxE3n%2FLIYvC5XEv4YzOIpInBI5pNUElkEO8xcRPJXYXaE%2FK5%2FD2RD9eHwlW%2BQosgJVQ1jM5Of0DAQ3t9ZdvSh1jbIGQzwilqjE6w5pA07Y8nPh6beYolAh4rxBkp1mV1qHpwub3MH%2BDZaR3H8CttlkJ7S1YsYdKa6VeEbA0iRuGcUEA8wrZL%2BvKHKmvSQ6m0rISbZaysw9eCDxwY6pgEJPDEO6OE3aYtAwipyA7qTby0o8pPzDUajyDeeZaQy6%2FyIcURjbhc7xhgg34Xr8nV4yyVMsbNqg8MRVSZEdb96eQRQ30z%2FvFVfmKEAqYwg2f6CqSiwyWXfoD59bw%2BckGuMyTCE8tZjy8LVprCz3Sd1PaNUofPrcD2KDjwpEpCMLEHvW6a7nqFVyYsZPtlNxXsdSf7JM0L%2Fm5gKhmhuVKbINQ6i3qWX&X-Amz-Signature=7a785595ad700f1078001dacf81d0348544ce80f707132d8d5706ee8cbb50e57&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

