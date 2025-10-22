---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YH3NQLYG%2F20251022%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251022T210048Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEH0aCXVzLXdlc3QtMiJIMEYCIQDdyNXOnL7ZmnA%2Fo2wXKl%2Fz1QLJFzmSMVAFdYVT0PTQiwIhAKg08bQg7yxt39EglLoA1m7YSlw0pWnCkaQzoVhGuKilKv8DCDYQABoMNjM3NDIzMTgzODA1Igyf8PBmDu%2BNjhnRAvMq3AMpnQYgAwDcJZ0W9NiZ8UPnawlJsZa49fS7bMJcAgQSna%2FxgynaK%2FopQcjy7a4Ng24hbaaoTj4a5YUUAPmsyNfMKC3l0LHrtbyMdFWbh5%2FgJF%2BSVE4U0NxBq9UfoJBoPVt4KCrp3Ndf9EfNqhlCn%2BBO7Stv3YeWZWplwJw7NsyMDRlzszB%2BDctFXjU7pif75uRn%2BbEiZ0B2om3Cxld1V0Pe2jIlS70RhhbK5S1ePCYrN7456NI33fqhBnHJL2M1dPNhLRcUmX25NUb3Qmh4eKc7m5kVQc2j1ZFWzoxtEanTNNyD%2BJ4Bk1oFpzBbK4pcznT4kDKKfKxSg8%2Fisyv0tsZfsI%2BrWqjBDyUx6W4i3YODS7OwxSfL7kyy0nw14GR7St298BNDNbBcxY47FXDZhgwR3KOetJv6iagjfpr0u05%2Ba62BszmlIss2Kbxd9ZEpPAu4DpGZ17cWIehnRoeZs%2B1tykEs8YB3t0gRP3UFGTjl9BD16QDwiX%2Fzmkr%2FAvEuKFuLz%2F2l8iwcfRRnvNHLxdmfGWOkeDUEnzMuhhUUrsYf8IZzXecNJYFSy5RA8rQMn4dUW21tq9GroNihGnnjFhcU33Mcl11MmTKnjhW8ohqCaH%2Bo1rK6PJk4fR8hyTC%2F%2B%2BTHBjqkAYYueqriDFIkrZEO0eIG2UYv3z473ziEtJpR0fYHWTMdJrdUBkhtpaD3pib6YD6vde8Q%2FZonuZD9b61tztlHQI63yDNCGCDgmdGdtRwnCmTB8fuF02akSr66G9jvsP07T9se18m5Rdmy%2Bv8SpfFoBEiHYvpxploIzezZ%2BgB7JeuIBmhzuVa5T%2FfsR8%2FjUHyRdV3gb3kqsBqm6P7CyI%2BRI8f1WXuA&X-Amz-Signature=e16bb17bd697e633f0934c335d93faa94c7694c9d655c840b5b67df5a1796f43&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

