---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB46673RAWY5J%2F20251018%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251018T130102Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBQaCXVzLXdlc3QtMiJHMEUCIQDWLThJG2R89%2FnxDSGKZQ2EAcHKKp2gvR6AjSbZudJSXwIgPWUrcMrUZR%2B9nFvEbvKg8dCES%2FZ600ZvI%2BXTft2B3loqiAQIvf%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDNG5Cg2f435P1IoX9CrcAzN4zsLH1A39OM9WK6ou%2BxLOn%2BZ16fHtSBLSQk4MpxWkhveXQdXY7PSNfWxNOFmBjVXFFXOmEEXZ%2Bw83sydkReTYRKLZvS8ThUqXhTfso6b%2FFgNeNxXexaV5nWW7fyj2y21LcjJzVb75sArURSEpu7AirbTPuggu3rCdP%2Fl6IfEOGOVJR4XVIcPF%2By1DkywrSpAqibCEVfUnzKN3PBTwAiD4xXTAwTFclHZQXxa3ouy9%2F9rM2jF9cFSgrccq09xKo2t7OzKHcxktTxU4pqDnnHkLyNFgLldjJyZPoemZXpTvuqtZK6ac%2F1al4VibVgLylLWD8bmAy%2Bc3Ouiby0qzGWPzsROJG0TCfM9V1rPRal56S4luIHiWgZkOseVrYBzHgD5chGHvnz1g6twYfrWVYYGeBwDotLRPTr2TX7aQKb0S2PtLf1hVm65sTdjOzuVhVJWKXcLCbMLQ9o8A3pIsrfBCaG5XA7tZ6W7oH59o8mzkTCyPsdO3ydfkwfoseuGFhlZ1nzphFWkO2mdE3M66m%2FNiF6VuqwwNpQSvbWCCRsJMqML7V0%2B%2Bl6SOeD2n7J4Je0wjDtFS%2BFIHs4GHjhzxJqHMVVPgC2q%2BDHwrWm%2BHO4Cxvgq0enPjNmtKt37gMP%2BCzscGOqUBJxoy%2BzHjPT%2B7pxOJUHS4KITKaZ6pEKy4eCWfT4x8wwYduFmr%2BvXY6FPTvdUnXE2DhtoACaL%2BNPw0Shv%2BXm9VJffp99G8%2FFCclsOcrAZNBXJxC7K4JKw3uPNxaaxvsf842Df7JtExidR%2BkiGF4glPk7lP9p3FsM%2FjM%2Bx%2BwC%2BFA%2B8OFlfyw7KnXw1eEn%2BJ9mTw7QmsfSClDNKFIMlsS%2FFnKWvvYdsm&X-Amz-Signature=d761e725592e82b830a0704dc8cb4af7ccbaf1a4c0fd02c3c60e28c3cddefdc0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

