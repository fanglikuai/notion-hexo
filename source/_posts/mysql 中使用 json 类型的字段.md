---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466TGDKKWNT%2F20250922%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250922T100045Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEKH%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQCjtU5IKJv4lPPrVodWkrQvfOC%2FthF%2BPPOvqwuTZVBhOgIgWycs3Y7Ny5hRq7NEYWhDA7j%2B1dDvFgln2qSB5iekywoq%2FwMIKhAAGgw2Mzc0MjMxODM4MDUiDMU31cHgWaVsnsnsUCrcAxEkcL2xT0z5%2BUxjrUDK%2Fkbd8udse0hoLlU2yJRp5aYhNbsF%2FS0soqEZHf1yfj05ogOkcx2uBjDyChzMBZBsVfMNMLHNsZmNm93GOccyjjTYAxENFEHRjyOH4zBw2vaWiKzpiaNqqLtE3RtK39fb1s9hcMz742v0rzDTt8B5hPqxnc82fUkNkvCkFHfKrQjpwNtL7FQzZBjgFoM1DfROvyuOGC3PMam%2BmwGQ%2BZjopgLDZFq%2BKmwju1KLT58Pd1uJoru%2BS4C3dcSahcgfUjpuq84dI415HD7eeYYl5C4wh%2Fwc1NiG01wwcn1nnZ2UTifyP%2Fs86tIVJVQahOP1fGUSlIRkIBL1Cae3zv5FayzQgNwAEu5Hpxe%2FDa5VyzlGYYYxRpMAFVmC9y3%2BAk7KEcq1D4hpFJUpZ8tZjmLAB4ZuucUG8EH5VqYVW4sWgdaftB5J6nFqGzRaqt%2F6tOCnWiKJfFHceQXVAxiU%2Bdr7KnTTYmQ1M09IK9c3tMDz1%2B0rAr872K6FJDi%2FfstmRANyg2%2F%2B4xg6Gik8aex8y4aO%2BDX6vPsKrz88w7aGYIPkJ6SAW206oBMSZXJtqVTe8UVPc98ff7Zw%2Fm4EZLKQ4mBBdLS2oSUoPEszteNbmGthl75sMMSdxMYGOqUBYrGk3elpqGiVtFmtOycsRE9fHwgUL9h24YIf4UL%2FGPoJEqPjexpRqeva2FTiV%2B1%2FCBE%2FD3lK9yU0e78WSWxZWqAqrZr65i5mRfSZhKejxAWr61CUXxu1I%2BoQOUwWWK4EQu5gujd8iRGEBFLb4LjXlxL6IPNGAAcssfIK07M0Zt43Z1BzKePu9EdX25cyzlBpM02C8saIPwjFdulP%2FtQBPVh0pn%2BK&X-Amz-Signature=76a0eb6cead98a60574628b99b24bcab6dff8188aa8d8af0fb4887552bf5dcf3&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

