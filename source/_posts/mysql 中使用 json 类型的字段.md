---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663PHBQRXE%2F20251123%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251123T060043Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEGwaCXVzLXdlc3QtMiJHMEUCIQD8iRHbqgVnAIgHEuN%2F78dkxBl5ncxk3LqwwaRpReciRwIgBfRfyikeBhrc%2FqaIWnOsvcfgtO3HsJISUB7VXmf7MYUq%2FwMINRAAGgw2Mzc0MjMxODM4MDUiDHyq3g01uojACBILrCrcA%2BiQszjlh5DhWlGxlX9HCXAJBY%2FFXQf3X%2B41VedUIIPYuXVg9mf4hpTmDallJEqxE6mgq5SVk1iz88iTHtZLcHLwOi6kj%2Fh4qtCC2JqDF23hXMicf5yRYD%2Fps9bR0q9uITvz9lvBNxtmUXnA3Q%2BgeM9mE99yxuvZ9b2SlVIea%2BA8HJSRYl38eiz1IwS1%2Fy%2FpqG%2BFQMe7pG0grpHtajH%2FFOS%2B4fmzVrBnC9jp2AohcZh%2FDYP8Iv%2BmdvQj17pUv1B5l96L49RES1vds2ZILt%2FmS69XNtvFjZoTgjcNxgWbbubJZ9r%2BDhimgpkt3Q6F8mSeJyA1Nk8Dy1at6L2pRG6HSnWgD%2BdtlUC6e69kjs5%2Fn6AEJDJoCpnJ81OOJHOySPHKkOVPfDaV28cDWDKFj7pujbFOg2o1UWMTJzBatOp97%2Bectgboo7NGaqEA0iAyK5dGtmgyOdI69BPye905jN3enToWZflIH%2BoGONKbBgEckSHzVq8NQsraEkl7osR6zv%2B%2BvpwRDKL4t98PjnB1R0fFdUoOiJNd0pmZOBXHAGeTWKQLEXxoG4C9SWWKUsfLaRLAmwZFxuHRSSn2zNWcMYk1l9IpXWjDQtLsIhYh%2BrEqDd7sZRvqT13EqGsxfnsKMMGPiskGOqUBANAAJtQp8Zn0Nbz0iuzfhVZS6WCksW1kB5%2BxK%2BihniwKo27uf8fx7xSL9zV0D10H1FLbp3UtXiosaAFtGisvq08cB2YKNtJQnMHmFB5s75WMTTKmbO8wgalP37TXSUSTPnzFrqtiDjod0%2FVDpiYBrhZ7zP8wUzsjNjPcnWqDy7Xey92XWO6K5%2FwJoWx6%2BC9TnoTLYlwHr4XpTeU2HCCwNXSzIGl7&X-Amz-Signature=06e81ecf9cc248730f217305cd5855acd76444e1454eafa4680de40f1696bf26&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

