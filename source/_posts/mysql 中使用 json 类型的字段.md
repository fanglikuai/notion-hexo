---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664QKTU2PN%2F20251025%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251025T020050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjELL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQC6LOv%2FZpPgDLuEFK%2BX0GCF6snRe0%2BK8eH4Dm0sYg4ywwIhAKr4%2BvtrLcGtWXwb4JksTCT0SE9cbb6jcr5SJuTXAB0aKv8DCGsQABoMNjM3NDIzMTgzODA1IgwJVe0BfdNcp1Xxpvcq3AOZhYWxX%2BDp%2FgOXZTbBSsX0WVPYvuduSQdEhGLJOuAwzHAfT4Xkxv%2BI%2FT%2BzZWA%2FmdWoonlwUPFAaWN9vETSlJvWbtKfkZvmiQczt13ulXIl%2F5IoKd%2FREd8moB7ULtNPBvp2d%2F74NRBZp77TQLnrsTd1cXvNQ5ZvOfFnEXu6N%2BXF75bDjwa895yzD7YdevlRZRkFR5kjpyooY%2Ffgd4LrzQ%2BM3QAJj76A7TV32d4siKJhCeCxAlHV16ysnrbZ%2BEYeL82dxO1c%2FdcOO3n%2FS66UxMg22oDx2VLmzUod7FPfPjDotnWG2ITaxHHuIsxLy8mn7BiP%2FdX1Ub4p%2FzZfnbLF56A1JBt%2FEZMJEE19Q7GLFAL1%2FSeLAEXh6%2FxbDoKAng64zc4B%2BANtrf%2F4G9NtMRfd5pLAyLZ0i5HSpfCU1USS6EYgrqTx7Ho9XkjX1aHCiF84GpWC1DIKBHPVA0k18xsSp4xHn9ldMJjiFUx80wkEgzoHqhPtruroPZUEZiQ7zYAULejt25sP9fNbxhe9KYM9amHB6CbilMNRGKbgYSGWYTyTatFlUaLDysm1HCI2QVyWiUS6F6ATwzhA6onPL%2BiUOmR7WlYRttOGaQacORUXJyhG%2BnOyYn7kYtJiYhmUMTDU0vDHBjqkAfmku5Vklt%2FVagXw6ptKLDX0ZWzsY5sGQyV3n1bnISKcJB%2BWChpO4HIFhAn9NT0dajpRoVt%2FIuJEBnBwQHUB8OyWGrVJEa8aB0gYxYvmPnynvtwn9nQaoYoZidUfjjGAyutPP0ozaSm9vjLoLrcyXmzeqEgQO%2BmJho%2BuFIKXnKTC8Kma9DY4ebe3Ub2mAN5bXjt%2FTFz%2FwIHC7V7hDga0SSHb0L%2Fp&X-Amz-Signature=7bbfcc49c6e12f3e2ffc4ed32432dfa50c5402ea887fd5623519713f865a459f&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

