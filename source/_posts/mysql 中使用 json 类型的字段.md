---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664AD6ZWFG%2F20251017%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251017T020052Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQD2TXH64zO3FsLp3V9PzSDM6wl%2BrUz3ll2oEj9YBg1LiQIgW2nojr6IUzQ8oAL28Zv4dujF%2F53NzWsQ3ZaSPTZfPKsqiAQIm%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF0fiGoJBW5c4evPYCrcA%2FxZT0Q2MaDkmgVJwsVPW3w76KF9aZFS5OGNLGWk4K%2BiirSO%2FdslKoZsanOTe2HxE0pf%2FGbePiJjjicYDckwQSgSbY%2BLc4t9uT1SW5S4uGvwhHrVXzBOkhVmkbAQdhk1YYJWkw6yB1Eyx%2F5yMgfvHpsDX7FEgKkCdyKVKj6OCf6NkI5l0zufovHenkf8W6BJLL9D8BAMb%2FAcj5iLka0Av0AoChoFHZoeWy58QyphkBknee3eeqy7iCdOpZLskp6p6fTc1jJQYc1UbfWnowTVepiBPgF7CdRaU5ER5TpmFHEsOFoLMZ3qtXljQYZoL7%2Bifw01sDVRi6VXzga4VN4%2FczrTOTseywxCsXJL21j6J9WcDRYlZ3WkknPuLCGImY9yplkVDJcLW0kBgk5p26PdGPFnqx8V4nmUAgfAmMidS%2BzyGb5ji9cQqV2uIp7Bb0J2YSAhzucZ4BmIcYsc%2FlBHtaL7qXxSjVEHVKnnNGhHwoe0ogOM9BNv5%2FjfYzdg6gQllJ510hNRcWoHCO0mFNA746T6xJF57uqcDNE0pr7qHauivsKiIoRB2kv0lOWyyQZ3%2BwGsDJVrmL9rYcVFKzB%2BGFCcZaUBMGwOADUPfgfsBYKnPdiSDYPfYTR2mTeBMLXBxscGOqUBhArngI6ixQvhOAibVngJUHyA04yxal%2Blp4xHN3Aity7GIyE9scvQABPQkhsEFZyORKRucHJgomMToSxaVwT89FPRJhUsJAD40057ySrnanm%2B38GnfK%2Bbbxci%2Byyj1sarGhhkcM524g5aygKH2adFxrzOUDtUqbckXAUnTbM0oJXwyfvCQzuXKu%2FGxQ9DKQlME1OfPyV43%2ByjjITm95rsOvadrlzJ&X-Amz-Signature=ec497ae33a130670cb6eb0a7c913c473f0935c1ec329089820a279cf4b44c506&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

