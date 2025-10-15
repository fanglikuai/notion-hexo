---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466XGDZ4W3M%2F20251015%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251015T130056Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMz%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIDO9rDUL4N1AhBbnbNikk3lL29ZS3p86UDxHgomhoOMuAiEAgLjvrl1q0J6npNsvSVXytj4uxT5RAusmJFSF9DumMLQq%2FwMIdRAAGgw2Mzc0MjMxODM4MDUiDF%2FGfhPYVprfvHwplircA98Ry%2F4Mur2Bhz2Ehro9heoht2urMltYexJ1H%2BS%2BWtFDShPybcCwCcmNyz248KZ%2B3ZTlyJ6pJ83XxY4YND6XvKPPKGidzb1mvH7u7xmTtKyU6Y2KcVsXsDtz5bGawNVZRfKm%2BkRfT9YPqbJYITH%2FkL4pXfkz%2FSvN1jxlehG77LxW9mjyWvXzwpemypSEycEJXOCFBss%2BKO%2B3cVimY64cRQ5SsvMnswECxgQcI%2FfCqDlIbAYhg8GDo0R4ezMrzD4kWq7KohTXQEEQQErICrzFl1T7fjYjZo09RU%2FfLEBSWwcaKfls0Yhw6fenFOj3irxytctH0%2BCdRxE26sP95Rr5dAMv7iW9Of6Yo7Cl0KltpOTIgoq99%2BF4Q3sRYSp25PyJgdlc%2BnZEMf1IySqu2%2FiOO1FLaKFXRJXVOuyn%2FZWLLR7pKllKFFO1q0i59XIEO%2Bv2ePO9cehPaWgQ9BEzeC3zmtolqiTiD%2FJ9MzGFGKYE%2FVe%2BGAMFerLmer4Ty7xRHp8eDuGNFiEPBgfyKRa1wd8zxYoZBkba1ypbTRBRvolhlcpU67stl%2Bweqb36gNtuoeEdxaoi1557BGhPCF99Ga1PJHNS2yHGqVi836M6PQvaoVlaC1M4DwA6D%2FBnkTVYMOiYvscGOqUBTjDfl7YPYIPZxwDuKDL%2BAZgUsHjqSeWMUNmDbwHQVTob9t9HpkIpkpzevjqScGQPr3s8qiZ%2FOfMszg8SMVSP6vH570hgQIizeIb%2B7nf9K5JyNFBE2dpT%2BZNEaPDdxC9eqgIA3e0ktWwruUupskmK0JdLJdC%2BJ%2FEaIZv5hQWAKyXbdCpJZNr1L91w8o%2BXfd9t9gqMy2kd49ohc3w4pmWQxSlo3pHb&X-Amz-Signature=a6bac6e3c7326a28ee9183e5c7dd82db00b3c29edd862c7c0472ca4ec918f79c&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

