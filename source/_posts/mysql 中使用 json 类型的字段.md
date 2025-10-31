---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466RLQVZC7M%2F20251031%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251031T110039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEEoaCXVzLXdlc3QtMiJHMEUCIGlzKYSK18sxrb8prIOqOeVKbfd0945MtTSdLaciarhwAiEAgtSdI1JPGrc1hRHqmXkeHHM2SecHWVDSo5wzqyJFtQEq%2FwMIExAAGgw2Mzc0MjMxODM4MDUiDEijubBrhoKyZvuPvCrcAx86DBTGN52H78M4D0e%2F5yOvTXpzmWbWZBbrXEbhTePhWsLMfcGyZc4rV1G67ScnAXCkTaEXHD%2Fv%2B%2BxP1PWBQBKou3Uo6wh37h9OuMthuN%2BIYIo53s07ZNl%2BQkTX06QSyviwlaI36wKTolyrI9TesAqdpvIXSOFO9UJSMeK3fQ2NuyOpq%2B8EAyh2LpvjuWh6mB%2FiazlShk%2ByHUmyO42KzbjAM7cnXHWV%2FvXhHhVwd8YnGeauwsObtq7raY1J0cnDctgGvvENjLT5tDOYWyPJTGatZVXWlCDuf8CW7sC5ZQoVVTTz1iTXlDK7CmToG3aIv1w4v6ndvW3HReGd7roccteo0KodnBh%2B58hPgtuKQP1D%2B0%2BQh9oke6icTeFeh7194jKhTR8KCpOfAgIVw0XXspKnOztvwWp8WuaqjdxaCgB5N4JBh3geEzwx0bAIIwdlY436H6LJFKzoES%2BTxQugGzQh8qRS5O5pRPVfImDxBsi0SM1LnkQFDWhnl7nKVscdr5cqOvKypRoBm%2BqKjFa1sJPWkzKynY6yY3fWIHSrBiPs4cIS7RIMe1Ji%2BQOxv9FrodNu2HqTGi7DwUIF%2BmBUUnuoLucgbd1eI8MEVx2%2B7gggQoDDyqqVcIRSc6d6MNaSksgGOqUBcaWN%2FdAhGQSLAbfgDFeHk13FADnRG3NYQswkgyOZzh%2Bhcy1GwKEF6yHCn0Yx3t9xDNcvIx7tF2aWnoLzgQAZnv0of5it33O80Dxf0njOk1Q0DPSpZ57WrMY6O3lFK%2FZrbrYUGBHVGS2f1SWnC69WqTMybZTUziCb%2B09Qw9RT5rPAjNPEvyUx5typBJzS1jHN277Aq9pU%2BGeFf1LZYqy8ge%2Bgox3b&X-Amz-Signature=fbc8a93cb2fee15da8d467b28bf05948a5d1f68a54ee2e99a1f3f07ff04563d4&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

