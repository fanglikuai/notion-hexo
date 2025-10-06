---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663J7LWPTM%2F20251006%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251006T210039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJGMEQCIBpCXT%2F1QXpQG6vq4OkmTsXDHLBDwTpo9kXvMxX97ThQAiBjFHADEqbx1vstGhBhum1ix4ziUevh%2Fzfl1dEIzWsTaCqIBAiU%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMc7LB83oQEr6UWQd5KtwD9Lwh7K%2FcNGKhoqn557I1%2ByaNAr4dV8578On0NTR4dq0pNrOMH1ZIFKRNVDaV42EwNb05xaxEAldJAZfmdN%2BxA%2Fgvm7ncZNDfxbzQavDW4z3%2F2Xa4MPaz9UMlTltflxcRELVDRAEcCewoabzHIA0yuZSZ1ePgZMmjUGv%2Fr9G6ZIkWRKfCPXdBlGgTc4ISlCJLGt1EcnYcAJYZ7rRLwl1CFkNdpgofYA8lbPkql2BMwQvNlRcLaob9gP1s6rHuDO9ySf4Ug9XsfdwsvQUkWWP1irhgDBjqzu3BomfhjLIAgsrk3Orz26HD5SrFToS1f3kAK4OGWxo%2F0PzCvCD2uQkYsJRSOJSvxy7Gf%2Bd8fR8zZ%2FfbNE5paBUfS5o0BiSY8ouKT6wlrsVogY2IhuRkyU2BeYGyjG9FsyDSHTCTZMn2mU%2FGAvhPmccOjDW98kHvlJzn%2BuCljHa7gxgjipdtO11unuQfuWaSeXxcUekGo2v4xVinU3EoVPtedtYz51VFqyXgVMfwiopGWMB0MzTO9grPwsAWtHQV3tV6ShF%2FLt29AY9eWlK3MkkvtOt2oCV1zAngSS%2F4ySsBUC%2BJLAGiJVNuD3%2FraPolYDgKagEGyi9TWXOC5y01OKHO5sx%2BD5gwi6uQxwY6pgG11t9QbaoTnPpDEBQbIPpZb4JZ8FnbcgnV8X0TzBtH7KAVDtEtJVnZdD2OWHYY%2FcyGVfVN3jOYvqYslmOXdkqed8EPgdo3ZER9%2FoFKlKmUFL2b8rEsNsNivpA8MgZnVHDaarTwsKPZ1FNqbD%2BeN43XogjoDeaYthGrAgc8%2FB%2FowQP%2Bp22JnMGiJHPUh9Mlp4%2FvYi3oVIjYNV8k4rjgeBdvel4tFfO%2B&X-Amz-Signature=bb8ccb7baad0ea3cd70a7283bb8f231722b4134f48b22fe5520c0cf7c21c4dc6&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

