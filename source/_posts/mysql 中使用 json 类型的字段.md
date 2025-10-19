---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4666PRS5DBC%2F20251019%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251019T020045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECEaCXVzLXdlc3QtMiJHMEUCIDivo571T7r9OSWxec%2F3gy9NImOIutpojHPXUmlhMnefAiEA0QJtHoFRFg6skauUm9K%2Bv3H9Gk6hABUOTdQjQxjKbjgqiAQIyv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDF38Neu9FYVbPcSo8ircA9ijYB2LHKlMBMyBWfU4UCmBEpplS54cMFtNq77VTRyrKZNPiIiplz0ffT8xHhKshLqaJVP%2Fzlx5fOa%2FyihQ01WugOQ05CrVxj5b7hgnvXK5DjT6pni1%2Fg9G3kwq%2FstbwIVE52vyevr6DfZgsTZ4YgFfih7aLjnWoWtnS8%2BMvSp2Z6ohXdhkdP81y528Br9paExch6Fe47wk2VqcgqxyWCRZYeo3%2F8%2BQNZfdF5l6WcU6hZM8LtzcF5LH6haXTf8ZLDvPBdIfR74PCBNyvgZx0J5ZNLT3Stm9FVuD8eTrNHWZ3dT1mu7om3eTjC22JmY4mEl2HznGjk9afLN%2BerwbwRjNBKPj%2FF7Gg1kIcKVcrGIEKzDv0%2FmoryoE0ww57pkPIoDxNFB193DlnIz9fZNdkOYkRJSNqu836TBabGhxez0GH%2FIeAvbyyURMryhRCV4LVy2IYZPyDpZZXXLMhX9MNy%2B8e6Cb9%2FDD3fQE7F4uc%2FbAUoxlIXcIBg1nPPGhRx0u18BIvrefQ5Ck3YdO%2Fvl77raq15XRtrghRMtpxA04ohaFhr4VA73Nq%2ByNrAUbn9fNuYpjogSfMPI%2F6otc3qXaR4fHJiQ10FLnz9mgTCQm2eBXJ%2Fcuzh2kSWYmS%2FDYMPPo0McGOqUB30OE%2FMa5g8X%2Bycm4aUDhb5%2BfxxDpJn4PM841OH1JndaotoXvaMcTixAPMwx5pX0SwXoOSMTtFx688LdFFKsbCV6stEv9Exu0jpudiiDFrdVm0nxhdDlEoF5nCXJyQv8IUR3Vq3HUBw%2Bs2yD2J9i2CKZcNC5UJP9B4vNGjCQG9InwxmOgUuEUCplBYYFR%2BxLucE2hVtfdK7ZZe2TMOhz2PMbTjtoC&X-Amz-Signature=faea7afe28f3ca6f1e1f40a36f835b871ce84ce7d3a3c344f9271134fcaddc7f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

