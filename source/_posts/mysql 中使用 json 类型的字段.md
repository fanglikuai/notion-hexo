---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466S4TZE6XR%2F20250919%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250919T050113Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEFUaCXVzLXdlc3QtMiJHMEUCICTRB7Dr8Ns8txzPUMvJttQ9ZTEkvX5cJrr2kROtlGl8AiEAzdsRU1LQbDIAWcS63qE%2BfCZCSZE1Tm2oOswYrozsthYqiAQIzf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDCOtlpy4184MhVgZ0yrcA4yuhv8en6MEnrY5kTJzHsp8lbdwZAwr%2FDiBjN%2Bd86fui1yP5AHM%2Bw5XjgnNb289k32lgQZ1DcQZ8rCMcqcHb2z%2FCaF56VTagTxLdrg85sg%2FkRS9oTeEXLheZAr3S8UzyuK9uYwAoR7dp76h20mgvgoRSbUJ6Iv5BwlLvIyEhdK42BaIqbhCJrM5ROj7HXOPZ9pVbP8rD8pKigSbzJIi6RYTZDcXC578OS0%2BBZx07kwZBk0ayOXUADrrwPjBpizg1QEAfX%2FFcn5nQTCuKEzIBSFktKnXd1KUQa8yrK0eB2gkJbkXRj9S84GwpWVzZ3f1TS6935T8X9Z%2BhUZkzYBcpG3diqeoxD%2BBmCH8Y2ogQXnfsATL50irTtluMmu5JlCW26%2BLbbsPnRnUTY4sOUi3B5UopLDTiayFsrr%2BPZkqO8YkjN3wpQ3%2BwZvY%2FGcQVZZot41kXLaXAJB4nATZdxiXIAaYssc81i0JMTMr3jRbl%2BjqApBDWmVSwbCZijamOXnOrg%2BkNCYaavhKw5muBdvwNZp%2FY7EXxPEffjotVXbaKVxNkls70Reu3Ba1%2BxnKFKenI221RVKXRiMTx8TLwdtFZKGWGVuO44ZVCrv7KPpn5JGoPKxZfdZqts%2B2Kk6TMPC1s8YGOqUBX1IdPBO0hYUKBVBI9KfPJ%2B8KV4oI%2F2lKE2LITe3ihGAzVb1777PZi0z51dOZokaYPl2B4ogPw12ilGp5pmar9vYc%2B7WupSQTYzxbhskaWIoOfTR3hSBjDqMcDbzdeccyLTBkqdBRxJ618ZRCUEEEdOsfzqXy58j%2BLHfHZ05EbCf35GS8Q5RG4kJUAFvLSYBv23JC5OKOkcLShprUezsNKBGbt536&X-Amz-Signature=5b57b6f71e47f19f43971649b759a5d603ad46047072cec3965eff378d8b0814&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

