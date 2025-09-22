---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QOCHLPRX%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T000039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJb%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDfZK%2FfD%2BVT3cLvUOmeHFccVv5Gm3WjWn1LGtNKaShOPgIgLVxiOOzaKO9GvRjlx4npWLlzlC0vn%2BIWa660hcBtzZ4q%2FwMIHxAAGgw2Mzc0MjMxODM4MDUiDIYQevxYwZX0y9G%2F6ircAw7VfwKSpc8ssmc9tWcmGsXbjsK65bHDp4cgrGyir8x2C1u5rQpJMqj6K3X64fiYcBrnvAOL5ESfSK7jgaZp4sM0R%2BWhJfyyfgdXSao24kPbUDKTanLxuYlb7BJjyoejqY%2BKjm1qOYHGJayZKSXLz2BxpdkdWlNVa1%2BpYoTpXXpWLEjLgb5ft40TEMb6vdp2KZfkYdfggWE18KG5kShcmCKl2ON6Wr6Rdf%2Bflni%2BvHuf9GWgem9H7qvm%2FYPKJMvioUXdng6zEssKc66QCHf%2FvgLign6NRbBJHAPdcGLsci7YOacGcnSnJiQxcWSIZVrMFxJ50HZ9jX3RO%2FrKXUGkqvPDYbETuTcDwJ4kLuIcPUtsZpabNxlquzq4NHSzIIyUGq%2BlkFraa4plJhpaU9bAA67brOlnobmi%2FTQqK53YFL1VV8%2FcG25p0zsg4V7G6ZJUIDUobi5kwLSvsL5CAFemykO%2FfyLsPBATJd6lLKfT%2BreGvjbdgR9zE95vvNcs4r5OqVJIDiYRmrONNT5609ZtVVm%2B2u3tngA9%2FOWPEAeMhJW4PBwpoVOKLaLb3PIol7GB15lvlWY%2FaUdlnS9QmV7z16ymPV8ewZrmwFjVnSuQAECWtuR6RYf7cvaVoQLBMPLfwcYGOqUBWAvUc8YaD%2Bd8%2FFFiJICAbyOV18lbNiB0PU8ylHtwQIvG7zdyTdX7epHnphd6Dad%2BLcMcYWCsIdS8BdooT2m75GFYWmHauU6Y5sRB%2FrNuchmtb2nbDIGc6bNvBMdDRhpP3Pu66i%2FVSpNYpg%2BGDlKpXM4SFqn3nbGU5d1F6BLMFlHv8U136tzTHIgwTOf6dz3CUAyKIsnSGlTgWwLrEf5mNQEl3cxS&X-Amz-Signature=aad6c615cdd6f4fcaaffa3f465699268c327d0513227097c61bbe71cb1be727d&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

