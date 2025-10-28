---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466T7NTRTZK%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T130041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAQaCXVzLXdlc3QtMiJIMEYCIQCOvn3vX5gz28DJiOcx9JN9yix1fsogpfImJdIV6sY%2BvAIhAPCzUCFbFLG7D2lrvLXNkD6Xdbz2oPC8ieAJ23%2B5%2FYRPKogECL3%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyfLdgTtpe4mg4Q12Uq3AOINgZp9AxkXJRnPE%2FvvDEVKxuzeiZ%2Ba28WakPQ4Bp7s1t5EtrwuDNSmVEKNBD4UOtUmpLS3KajmvQIH2zfa7GwaqQ84pTRsvRJ3K1jDB1%2Be49F9NX9t9NzzXMCUhL8lY06zAnhpekY%2B9YKXnEvibYwoS%2FWePE6oVhwL58qJp5AR0q4iyHkrHyuKZaOSRrAztk1FkfZnbtAU21F9Gw7ZKvLhL3MrJW%2FdVbe8l4LwljSF3ivE%2BdUFAneKjn%2B9PaIg%2F6ZRV9gw5tvWfLsUiTk%2BTVarhZ%2BISKb5OwNgUSjYGjwuWh5Fyh4nN9fniaUEILAFqlgtLGntPwsP47csoyr0u8hQjBNQqn6%2FN9CZQuYv%2FGg7u3VzfTExA6iR00jZLfJbDA%2FeAKLzvbJI1l2z4p7KOBcFcIvU2YTxIgDM9E4XPWLFjQE7DkwSadfgJ8vg3a66x%2F%2FvUw7CFmBXDTc9e0MAcf0nyWbK%2BNL%2FaNsIR39g5lnTk9KIJMda6FHRKeyYOg7JL3%2BquaXjNN4F2Tt%2B8D1SNfDFOSo043ODITEwVTVZKc0nEN47VpcKOqRc44yKu%2FroOIhj6D3wylODW3%2Bcq9eS3nCA0PyNGmoY2ko1gmWj8WMPUYZx3pgBVFTSbCq%2BzD85YLIBjqkAbXhKg%2BwvuMdkqw4BOFnNrguT5NhkTY4%2BSQFFDnWv3wVtVkNP%2BXoHYBA%2Fcocx0zkChupnCwqvEan7lDafPtmLznZ1aBulV8uSMCMkxyLzVBrnkM6bmL0kW1Ag%2Fd9o3H8nIlV3nItR7n9%2BNjzwukfUEHIQQVQkYBIaH%2FrUgrVNpSJ1OfAvBhP4Xn6jsAYbks4lDHbwnd%2FS2mqks046na892eqsDp%2F&X-Amz-Signature=6bcdb00895a977db5e6a04d28251ff8623519968562b204ee1885f793401e967&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

