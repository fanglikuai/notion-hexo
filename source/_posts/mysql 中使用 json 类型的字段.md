---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4665JOU6ZQ3%2F20251119%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251119T230040Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB4aCXVzLXdlc3QtMiJHMEUCIQC%2FdpBjAU%2FB%2BfHSrSz6TCwNWNtS0IK5woEB8FQBo3Q9CgIgeu32%2Bi2wq9hfDrEVKaT%2BAPPt3jbNAX9b2l7Jbv1r%2FoMqiAQI5%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDG07%2B%2BLoFeGRvCF9dyrcA5XirEX%2BA8cTC%2Fe4fCj1InLP903TOzswBbpaDoaiz3O4aF0JCu4%2FUjPmzhptyMs6xKCJArzJ0bsqgxBtne1qFcjB%2Fcst0%2FzT4C4iBpQfdbYnAd1Hn%2BP5V%2FdhlJ8E8wSKkyZyf7J%2F6Wz%2FER6yw%2FBJdDuw1fL4g4Im3hbIIFbj7%2B4SpWLZLAk6iYqs27zgZVZW2PeMZUkqK%2FvSckYzp4pffYb0%2BQ07UG%2BRlb9I3kWPPnP5k1UeT8o23WGkeEogCpH2S0IE1O45GhaDXsdRfzb9Ve9n0cOQTgu4Y3VCTCK3lIBIZlzqr6N8Pjxow1Z3LdOCSCeXaEly5%2Bv9%2FnNFe4gL%2FkBSTgVnp7mCqIdRxOnFNnD%2BieA3WIT41fWwSHbu6qfkNDC9NSYnkP1%2BGY2R89gQ%2FZXuZ173NtUe1tauJdKSuS%2BQ3RV1ZeahwhVbk%2F4vYRaiZw64xdbLfPy69T9VBjXTlOvb5Cd3li%2ByKtEqi4%2B16ta%2Fsve5EUhRNPxfaATLNcgR312BoareTlZV2m2lLXD4YTpwiP6NN52GwR%2BqwTodRs%2Bl6SlvpLgElSFg9%2F7b58ApNr0sU9WfIx%2FGRrR4mOhy0hcX8KwBbxIoGAkt6ewDM3UfFo59%2FGMW%2B%2FyzvSD7MOb5%2BMgGOqUBBWlmoNyDzmk6pFwYgTVykCOuISueU4U3db8WsUsyWveABNZZxUCgEh8noh58D8EXSvZhzlN%2BXKw%2FWVtn6vV28LVcaojVBHtHS1O%2BzROrWBhW1DGK%2FSaed34CxGZLVitAnzVn4RzuWK0%2FeRkmYFn8Hc12lhz7sFEuNmkIsAxQuQuDBnhL5y%2FzBmlFLKZpsYDLgbwsnOcPm43p%2FlJeiROWbrP%2FKfY1&X-Amz-Signature=57c562b0e4b63753588bab5b299c6f6c591ff09b5ff69f633a276322c5e9644d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

