---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664X3UIDNC%2F20251124%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251124T090046Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEIj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCnxl%2Bs0rLR4TCtSDd%2FXaTlDpVMF11%2Fq0RjrrkWoGW%2B5wIgf2uK05r%2Bffwt%2FlJelbJoPzB%2BaOfZ3Mt8SZu%2FOkhSAPUq%2FwMIURAAGgw2Mzc0MjMxODM4MDUiDKczavwspzpoMJYVfCrcAwAAEt0JlBLX1%2FNxoaZm9kxqgTc1uA7HlWvyCBJQB8e%2Fzuaqvm1oBDKOl5K8iR70GUnKTePY5rhItPm0CrMQ%2Bc7%2BNKYBDC6t6Em9%2FCjkC6fw%2BB0wwwZd6A%2FqjIH3RgUMyViApQJbzUN2aXlGZIygkEExWnl9%2Fud%2BkUHSxiU5Nd7D8h%2Fowv175bZ3a5zHRyhjf1nHUkva8AY8yFQDPMipzKSnGrjNDKw%2BD9XlLWPg4BeB3vavUx1PUbBaGf%2B2%2FVCM8z4FBnba%2B9TklxOOfBxfC%2BoTtbrBzm8E888ZNsRBpYh%2BD%2F9gopVlTNAXlPbJRIu8oiH01%2Bvci8iUtt65bxT3QByBVDY9Q%2B0EaORwNCRkX54ohuIjohO8y6miYkzIIuzmaDACiaGbaltNWR6MLWxXix7hkNmmerfrq8gfrQUCPGLJUjI35FNjCVjHHkXANUp43b9jTBKr8j5XwrvwO%2FeVTRILGlNzNmaVFOZ1YO5vZircD8%2FOO4ClaAkcYY6zJin4DHBtK9B596XLM%2FwdaAGNcbQsQelYmLTdyiY0QCGrY0oFc79P5%2Bx07NelJXzT8KWFejREbfaxdsDWJJBtOSdMkPr1LjL88I%2Ff7coF7HZAktbbA7hiALvoHCExWc%2BdMIujkMkGOqUBaC4eFYK%2Fat1b3sCDefNK%2Bu7wB50f8fffK2kNFGiPqDOuQJSBUECVIycB6oNfQ1zutlVE9bo7eZvA3MYY0rEDsZwxAndGujqjEB12sRr7XMCZuRHX3UUmy2EzP0yE5UqRZfi5lNBPp6jbZKrumP2%2BUv7XDchzdn%2BhJbMktCDUQWKkf6avIRWxKAhhvAAsIXMFXI4OCuO0aH9KGKnuwbDu7%2BPe9YYC&X-Amz-Signature=4d50295a4a0934116c7e3f3768c319dc0c6fdaa06da24bcd106dca8cfc51a9ba&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

