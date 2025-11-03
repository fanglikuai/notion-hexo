---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665NA3RVHL%2F20251103%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251103T150116Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIAd3uPSxm5%2BlBMKWA31XwcuEDOjeWuB7B7JuqsFsGlc4AiBNrXiasKHnEBfoqtwErswxTBeltLA8g%2FFKErJ3Y8n41Sr%2FAwhfEAAaDDYzNzQyMzE4MzgwNSIMwR3Dydf%2Bu6G3d3kkKtwDb78lFoXRfJ86I6ibn8HL2UczAAfgh5UZj2ZazXcCc17LF5lEnvApYvuxsHTO4w%2FjpXCcN1Evm9kSF29eq%2FTQ0HOuYIzqu8urIPRuIjlSUzLcSeErXS%2BSsiNYIduE5KgL0RlVWGxnrm2CXSQzRCaVFOuh2LjiH%2FJLeRLTPhWk3mFC2aIjVLADLOlr2u9HBZLCy%2BK6UX5iH%2F%2Bkp6phXvwNIUW6eu8Tn%2BfNE7xbRulW62nrgO27x4YA5UOf5U6dwFf0LwR5%2BkZ5GVzKJlod%2BC%2FAciX%2F0FeGgublAC%2FayG2lFZ%2Fzd0QU%2FnFCMHXtUo%2Fhr6%2BimBAJJa3Wp9J4EAoGOlPHgCnrbI1SWZ5rshEmG0Mqm38k0YQcLbc8zki6HtUEpUS4K1DokwR8FQROAHBgDbctlHbLKGEC1kaaVslPVoJfWoV2yngIkUj26iqeMwaRymWcnwi14hOigG3u3nDErei8HdiFlU6lwCV0pj%2B9JWkbylJ4b7c%2FEXS5gLzD72sFf7Bypyfbw6rHV5%2F2aZoSBjCh8D5pUTizmxYb8MOMjih9bHDQiT36OPxq%2Fk5zZI%2B%2B5fp3J3OPxe2fbttbJp10xP63gtLJPh8jpUgh1zI6eBorZghVkB%2Fymq5ZP0s8I6owlu%2BiyAY6pgGd5PmoXdBBUproJOvOV%2FK80lvwNllebuD%2BynO1Y3xo7N0S%2Fdx41iEhDXM0oIcfxK8BbnquD7wOfKnrZV%2FaRyvebjTXb%2FpD1BBswx2iGCB%2Bf4q7fS%2BeISuwjO%2F%2FfiR8pZgRG6Tu8FsaZuZ%2BCQacr8ARtTvscS9yrYUtB18oX9FKvaUnbyRDN5tbnVF0U%2BiI5sJDQyXo9HcweZ0PjROeWSBKirIai9dk&X-Amz-Signature=f37337b1e6ac68a4b59bd4bcedac8391d2020a647fd6b877dc001f0098f71bac&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

