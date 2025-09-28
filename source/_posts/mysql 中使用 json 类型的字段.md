---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665PZRXHQQ%2F20250928%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250928T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEC8aCXVzLXdlc3QtMiJHMEUCIFinWHJTrLEXglPFDKZL2jG1yu5%2F2IfTnpInGQtl9jQyAiEAkCfOqq%2FLOWFxGqazD4bWa916mBqUYCWDKD%2F0BxVFzrYqiAQIuP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDO617%2BH6245SIItvtyrcA78nZOamn9II3ZKoM8gJkbBl3Sla3qwqBX2pzHCWaYeX%2BA9lbq%2FZZwThcgSxq%2Fsqnr%2FCWqnD3sMXbU7Rglvv3r5qQ6ilXbab7chqK%2F3suc%2FYdXQ%2BIOPX%2FOSIzJKxWl0tnCJspE6cosYkvQV%2BP8y75l1KwKMMEwzCjR8CIqkfVLQy0IcQvYfYfCN3T4JCFRAIjznWXJ8PfFehbE1kqfMQJrGiztuf66UC7OIlKAn5UCryor6oEmWsH1q8mDdcx1GI1Qsvqgt6AWd%2BvmOZe8s3O8rhOHbcDfFfPceTVOKEybxjsTKyBSff8Nsm9KSnrCUvjm2nnum2cya0%2Fas6veiNxpSO%2BdlsosFqzqkFnKQmD3ovVotW%2FwlP3LOrW7DoX32SgTMxWzbPqNhNXSPAOZQYTZyEUrNCYWdumZusJv9KrfBwWGn6AnED3Gzq2bZlkA4YbkSfFqWaLdX%2BEY9qnNZLnGwL7qyV5STlTnEx3tJWJ09jgFj8A3vqYceR0hp6YqzWrERfJpX%2FCysaohvQtQtdeNF5V05bocJY%2BCtlhZl9FXN2qE8Cb21VyVhKeznjr0050meOdwuUE3QUEUMJ1p7pRCylfe7rqnceXgcYbfnYidXsagAPM0U4n9KFkF6rMMS848YGOqUBQApt8Uqvax730iI%2FyMfa1OdALYubgV%2FcrSjvZQUTAfpsWUbnqjWfKtWckxzPanQXkIYngHbdZhG82KX0VWVrYnTj1F4EfA85WcPW0tnRr4JQYpnhoiDQ2x6gi9JgjyYGu6QJdLXX9BkD2kpboZrTsAzIzijwZ%2FkWjaaCC61G5X9RdnvHebYXAae6lbi%2FysI0feIq5bdBtlhzEOKKslsHP6X%2BnIyz&X-Amz-Signature=e7306905c80a9a149f1e5e6a8a8e470670cb8ac4e965a4d78d69531994ae28a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

