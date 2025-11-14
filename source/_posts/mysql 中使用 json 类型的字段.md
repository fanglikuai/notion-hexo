---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664PYMLA6P%2F20251114%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251114T060044Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQC67Vp04kEW9WeXSMC0n1QMms85%2FI9dbEVu2Tkt7bygfAIgUiJRXtD0t9SZmvDZB5799lMd3oLDDbGnsaoogqICOg8q%2FwMIXxAAGgw2Mzc0MjMxODM4MDUiDOFKNaC7quVx%2BV3d%2FSrcA8gOzHUBuEJXr3OV79NyB44c4C%2FEAZ7MdqHDC%2BPSenYpYxwPk2pjrrQnsnVPrARVhc%2Fu7totzvhzRLM3WOGAsgy82M3jo9CdADFOiyfa6rufsLNZTmUdJ07QLrY4p3sw9X41UzXiwYwLs8CQFc9iAn%2F62qUgzuhJbBwpqHX9jORIrrcqzYEPaXaSCbSl2CDCUFwFLXQU7sdjE5zJEE60pN%2BlGbgK7Zd5n2HUVJvXXZ9lB5NaynQagCtqlU49%2F2jWNvS%2B1%2BmUB7b98bHuSKu4Gp1QwXuSqx4DB2n7%2B5SsSRw5HDZRVzcIXCwQ71SnBVcBpL3pYqUt6D3%2B1Vd6BcGh6l6WdNKFhfN8v5BqImVkzVYnfgpp4ueAJ8Vjj3LyrGf70JayHoaxT7iF%2FovUfvS8gebiiwNQaWYVyojTWcWyzBZwwcY5xvJJfifjeHNp6lA0pJDemy7c%2BgbYL8fVhPrm6LUsLnIx%2Bvg4R7%2BIxyrlfHxLwyzmsk8w0m9kE4yCDxF0xOnHp5P3zHIwkkkws8txQW%2FIDVsI7itmUfKmqCZYQNoH8TMM%2FIzL%2BZa%2Bkq3MvOcf2OkOgvkqlh93BusaPvO4BV5G5O1KRNM%2FmM3b2shIlZmC%2FqWm2ZKgDFc7e8hFMI392sgGOqUBNODUjCnyJvE8iBIt9NdhpHoVh7GbzqY85g%2FHhe8x2rUeMpj9Uf7Dl3GpVaxNHt8ck%2BqCRFm6rC3uEXTLrxLRyH8mNCZkN0BC5Tssntyc6UpByWgP0cpHDwiDIQ%2FcoMeQeljbb2MkNv42lB9tML0Uk6v5ovmsyd8vM1LtoVDeX7zqcMCs9uInVbzWnSk5fdws3g6kPOYbyaJCfOEJRqr1gYzXKT9V&X-Amz-Signature=a8fcb21e131ade080385c2e6184f0525b52b5f8c2d1d38ef0af27492c0615567&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

