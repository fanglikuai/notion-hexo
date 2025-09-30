---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466VMMED5KY%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGkaCXVzLXdlc3QtMiJHMEUCIEwAn%2FMvuSNQzfovrFB%2BzzzWhyNPMsq1fsLM6YiCdY5%2BAiEAqhQfzKGFcRdC1CQ2h3M6h1QmWSHBPlpdlqFf1Z1TbtwqiAQI8v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDBQ7LSuKREgcaDPpMSrcA0Xffcppaat9xQryPiccR4RlspV2n7UDGcRbqjS1pC%2BXAFC8BWjLU7TVtKVPsyAw7VsEkb3AKxzUdZrEKuVNd6VKQL%2B80oRnjrBQ4vyLbjOCQ1RHnIArysJkI%2BSG9S3kL8oOD9aScTdZENM6kDvHfSKRZ87VYPvYngEf%2FBUinkOLpiwmeKhPB5EslnrkZudK%2BP5Q2wzEpS8FTtUw4EC7OcKACpfgFUS4ji2obsTy8P9jLo1EUCUr0apifg2N2hKEZKFz2t4ScpMF8zpaMtNs8Ra%2Bqk8EIKUA1LDmndgbg%2BERDhlFWrHTNvZiW915qDk0kbVq2twvaRWHmfR0D67ikx2GcWOWeRQYR0xuuVa%2Bx1puD%2FqlY5csaoooqQWK58o7d1I7jjw8mQhMNormaGBizbpeFRvSPJPRJD8ZRMhipnp%2BCw5LQDRot6l080Yf1xlJcEx4ZE%2F4YsRQcGyRdi2TY0Fqq6obj%2BghplKjuLMpf29EtIorBdnN93VbQctjAsEI4529up%2Fuq9JC0SmKUVM0YAzWE%2Fg8mdhMqe5IC1oKeyCQibj71TzqEECAfbJSG3KFj%2F4EWTt0rE7eK1Kv1okwexQ%2FWAgP%2BaKswxe%2FVEE3N4NTLvtOqOPAYY44lyKZMKKX8MYGOqUBiXhIVoNTstAZUGmD37V64lVasAUOdQ2rzPLhkw3039Zw6L9shicgjBwLbqleAKqtZBYBLWzXNQXk2n8ad6FVgEjiC9kvM2MYzO83t%2BtBC8BI%2FrngSEdHfbsTDRkMPJDAZ1RROkcCe8PTd%2FKR9gTofJ9kR2bqWrNkvk4neavp8tffzjh0ZlIUK2Rs15qoT1uS0c36lHiVzEAa0nWYmPi95W9C0hKB&X-Amz-Signature=0c8328339df04a03f16179dcc532e53cfcce36c8e3888ceee5fc9bd4ea3d16ab&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

