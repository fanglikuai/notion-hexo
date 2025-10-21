---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466446KG3C2%2F20251021%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251021T110045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFkaCXVzLXdlc3QtMiJHMEUCIQCmMOzcRmxlJNkSjv9OgB2lTq82iochha0M7K5ZliNCeQIgWqMwkbXgj5dm7kicKb46ZymEI2CRaL5LkvMWj5x1rFwq%2FwMIEhAAGgw2Mzc0MjMxODM4MDUiDOI4h23pg9%2B1YzITHircAz6vxD1fnBFr25N7Z6hbWBp7%2Fa8sxaTv8ZWw1KKiU3HrllhaaBsd5adlbXaw9vzKXyl%2B9MoxktHzVgLqNqxYm2DTalyBqTnwlpb%2BY1p79SEdnSJYDWcPapSmo4VSzrpGxirISP6xwGggzKCnuV74inLkaWihkhgCGoL1Af5b5KQN16%2BUUAntVt5Dg9WVTRiduLMUB%2FWVxv6eqrvqB29EfsEbajXyzCHyUYO%2BHyCQtGXJ1c1QnzIl%2BM5M35%2FAyISQ3YiXogwX6RtxeaNW1bgg7JycHqcu1vqxkUXfPUQJ7m9hNYHkrG2pTFsFJ002EZKdWsS0vJ1AITDEA9gBrufsMmLh1Rm6v02qoBFdlvFG5%2Fxu7z4C%2BdHt7FowQq%2F0TDwa8KnAFDQwy6sFPhay4VvqfITgbPU64nFBkQ%2F8RFsjnLccXiAjyqonuUYm%2FlqMow6bMM9pzluxv24xj6JF0zs%2FEZnz4sHEZoouHsNIeUML7%2Bdi67BPXUEZQ7mZ%2BqZ3ntwa%2BY9FDObVEbV%2BPfzIyu507O3Kk8nhErkh9rdb1QErpijU3Z%2FHscm74x%2BdI7%2BV%2F5Y%2FjaXCP1ycP%2BMiyG9ytKD886b2Im2MroFfGW0p%2B%2FcIv4tSBavAHfbL0thWK%2BFnMIKU3ccGOqUBgbge%2Bb1i94IX8u4lcq1bqTkKndYg65vAmCfQGNvbsgVjrIt5BeZTtEOML3Oq4OWZHbV6b78NsSygoCzFwZYYv%2FxTdgWWPWkqA9dS9h66t3mBYFEZwLzFuEy5SFMvCBR4xh%2FliVCAZU%2FJFX5JlADtAvpOce54cMXYESRXzFQJ987b8AEXFyNkACnlnp1CKNfTq0iKUOr%2BWOfJosICyIIpgX62YNGg&X-Amz-Signature=a68fae6cdf210232286aff950d1fafa86c11c748db8f5d5c87642f1f8dd1c0f0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

