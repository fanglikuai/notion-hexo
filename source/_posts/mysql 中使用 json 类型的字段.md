---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UDOPS4TS%2F20250929%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250929T050041Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEEaCXVzLXdlc3QtMiJHMEUCIDxIa%2F%2FIrZp2xRHTZK3F8gEjQzGhhornFLvQJRQPkj0bAiEAljx29AEEjCcUmw5L3MxiofZKUUx2VfWapPi2GE03eiQqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDIilQjwNsMfdNcx1NircA2YI0tIOhPrx7CDRGDKuzquzhnW0Ahvcxbnh3CU9aVLAr%2Btq5qTliLGe9gJpYfH67ai%2F27YC6V2IiW3UmvYn2pCvexNQYHrdPy2bApO2qZvabHQq6sdWvZsn1dKX7Pb4LLcK9tFZcIv2w42SHucLfGOPRlTyujtC0FB4XrmVyTwV5ImXHUf5KE068I5k%2BsbZGNUBgO0vJi2HVirs4Q8%2BMsThGEHMKxCclb2IRmZqlAl6Z3O5GbLXodmaTL%2BdMHKhhV6b%2FhqaR2alTHE%2BDgp6yx6zD3J%2FzVxRVx2rdmftLDsNG5lc5DKuN8PWeCsid0iFfaBwh6ihAjtN3h5W0BxDaVzYbeaWkLjhOJEavS9d%2FuiBKt9F1EY9FMBPEjJtXofIDUk%2BYtKBM%2Ft95byKs273u76GDsisZayZM5sJnse8bnvXboKqSY2Ov8KS0gKuhyAOOJZXkL2AFInpObDhTlYHXalwD5IJNWr1DADVtLXA9hkyvEh4mIhdS5dSYy8w4FyRu2%2BCGKaBtej086z%2BHwEjPPwL5ljAHW3qsfHE2JV6Q67d%2FxaQrx7KDbJquiQi%2F69DNp4c2LU4Ddg6rcYpGjFngOy%2B9hf3KrCho%2BrtXfmFjbUWmfeMK2fZdr%2FKRZI7MNOq58YGOqUBJV%2FsbFP42cJf9sSd6ZgwZD4xO3Z6bavg%2BccrzGU6qdtKrCZDzqLmurpDbRvbGP33pOkfGOCs9hVbDSxXBGdiBcDkU1rGcwoBvREuA1zNYPKaXKBWN95IOZ2qQzv4fsbVxvfaBp0XGbT64Pk%2BbjsqI%2B%2Fe0o%2FVgdOveR3YrrdZYy4dRNZa3vkAaPaBgfUWaicoyV4SExGDWXK8PJfv3NjCAEozrz1r&X-Amz-Signature=d249c219a11fa70ee1175e4d579c8429a44a53a32addc6540db9a0ed01a02c9e&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

