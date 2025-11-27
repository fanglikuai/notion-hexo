---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466UTQ2EGIY%2F20251127%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251127T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEMn%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIGYyHSA3jZczdrRxbVSrfPf3OyoNny683fhgHiyhhnhVAiEA6tcdg10WOCUbPafWHtgeB6IYSD2haVDyp406vlQVHQ4qiAQIkv%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDOt1LMSfAZih9DO4GSrcA2liLvrwPVt4G7IstTcnytH8238vwC1cyl8cmhmoX8Uusu%2F%2FDWVvbg9TCoIXZ%2Fdo5wu0vbzguIuhGq4JrxHG05fFcRKW9GrGCcUWsnPkM8EFsX4wBxDe2%2FrykMsq4NJHOQ7fxCTLitm%2FgsdWC21fefyZ4AlT9i2F8MpvPOalx4BVrgQ4E1Vqiq8B7a11VXVf1j3Bd8vKQ8jV0VtXl3aUy%2FYDXIILMfnEer%2FXzncDcCPzYVmsbNNQiyha7GSho6yMRn6rsLWsE07FwWo51Rqy%2B3lcDBLJPbGIzPSblqsTmdLeicTRJJ3wOdGoFtUkXwyOGGj8XdY%2F0Fgb14Xhbmw1KDujBqfml1NRpUih1ayDP9eOCcSIuz%2BhobL67VYNzlbwoDQgvUayKBJ9KXMgH%2Bria0owvGy5VIiRRz5xI5MLIMz0maQ%2BDpbDD2EuHJju0BTrwJiAYo5zh49eJCsNFmWXioFigARfPwBliH6YiW01Cu8StIR6rL5j2kVFczAnWutOXBrcDPoRIe9CxY4iCcre%2BcWBLMqrbSj8%2BNhAFphEyvIaX1qzrNoIk%2Boql7Vx0LBjhg7JlTnDY3CF4bDiA7sdgV8HUzgXPaWWd7qxuum27h4hyhSXjlE8Rd9XWIH9MKO4nskGOqUB88isa8XrHbvmlj1hmROapACwvT7d787Yz8pyp49IW0HZhagzjz2gxGiJwU%2F8q4U0oDnpL1AhQEBq%2BH1ftJQH9lWJjhVkUNEgc9dbtYcZYRYNrQC7TZo21FNfSkO1LqO6Nhvqa0W2cdfAcaHQiG44Uzm15VwkAr0xMVGp0LKesAC6hJHzIb6iHZUeBbGyptuqjEIOMAK2ptmMzLNT5iub0Kr8wL89&X-Amz-Signature=1c61e694d76cfa86a8f0d87ee14d5f44472108ed204929c70209163e62c07638&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

