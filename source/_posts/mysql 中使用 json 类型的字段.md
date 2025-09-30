---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662SDTQKI3%2F20250930%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250930T140103Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGYaCXVzLXdlc3QtMiJHMEUCIQCHdryXGkbwOIhBo6NJxCeJaO71nNPdePMFlkF1eOKIewIgLpgnQmP78tjq5RXqwdJviluXUyQJhh2ToKJYOCbS57cqiAQI7v%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCygbbrESgfW4DxOFircAxSAsP6TVCk7LsXNdRqk16l%2BnZy%2BpgRd7ICP099Ey6qnv6YT%2BpwvGJLANuC0bXm86BiraH3eAb%2Fk3lSFF4AraXBlFwCupBBxL8PvKlrWV%2BKT6tPXaCoAJc5SOFOdmOw%2Bk7H5aR8qZDOzayLNluDwg0GcEs34NfcIxxePlzZwzkgcEfY67IeWtTrgKOWPaNduBP7FV%2FaVYZFGD9E05jFYwrtik8FwTMivUP%2FlGji8Rw5ptQ4xN6GJRqthBItrI5OB7BQMqNCBojpbQUBguJY%2FNlrcKcdEn66YqZgmDgJDKMUAxzav%2FHLB02DpEE1zWsk2ZgsQivOTLuzriKol%2B08eWSTsKKAKf2f3pSLSBI27dpnJcLX3C3EkwO5H%2Bu6MpPwwtZ8wafI46aiPkBfeA5mss5CoeObwIVXwMK6QygSSi2kx5dfAMghHViTpmOfGT1PhHivPwErk%2Bkh3PpuUPm9HcWFliQpYuYMFdNzaojp2JuI22p%2BKFAbgDHrBuQlVnXydTuearl%2BObXnXU4YnJD2lYDDa8Z36MJzEaS3jtcNJJl0dH8ndwDB029kxCHJ1qskfwQcHT62kZU7zmvS%2BhKMrC7DG%2FjTkMY38IuB5mddWzJ%2FC%2FDP%2Bw4xGu9%2FaOeXYMJuy78YGOqUBRe1xmFTEIYFp%2BkScGRAIf86rHk6SW9M3GLaeuMedwCjNoqdCuARBKBvFAB66IjITJxE65j922gD%2FdYsgf%2BHG2NlOOiIbP7UwccVmiz0FqzTx4cWz4s7tnTYTUi3BOQGg6xcRXpndk32jA16OUGxJnnI99W7Vj%2BeqxcstngzjfPyAE1ov2XVpeQF4kM%2BME54672QpgKWQnLZGdnud69ZuO3A3B5Ff&X-Amz-Signature=d7ce47497128ef57686ceb12ecb87b04cef960933147fc52df246dec2eab8e89&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

