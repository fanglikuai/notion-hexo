---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4664EYMOJJW%2F20251009%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251009T020042Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEDIaCXVzLXdlc3QtMiJGMEQCIBABmpghBO8x2abjNlrfL5XauG7w6TENFmedTbQ6ZemrAiAPwAO3eh99qe%2BtcHKPZ7M7qNN%2FfXPReq%2FxKQ7M9uLQ8SqIBAjL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F8BEAAaDDYzNzQyMzE4MzgwNSIMWuwfMrzqFGg8SgaNKtwDAAm68rJnU1uUjHm1vBF6KOJ8nmudcq9gbWyb3VakjjqhHj5jnfPqeYz1PNOkhQr7MDBJvtpH1JGgoZiOXp2IlF%2BLmOm7WOKG1u7iB1%2FyREK3wu9SVZ6imw2QOMRk%2Bxh6ehUrbvD1C7JjVl6BrJhOHBMQnRQMVAJmfl%2BqfBTJkz1qb3iWARRV8SIzVH5GdHZerCr47PH5e0ao2OnoLk2SLoPmidATCqeywOZd79fIjALgXpUFmSN7lBYULQj20wXsmfa%2BDVWvcBWypBNgDCJ12elbUD839wQeoC3iDBSmvvDp57XzZvK0M7M%2F5tTnG%2FUx81C0Q7xJHW76pfjO67aVqBGxBf6i9XTPy%2Bme3UVohFYMaamFkR%2BIbOv%2Fb7QELVRgfhAZUDMegT77YlWJHIr%2FQFRnKN%2BZUGQGi1rYGE%2Fns0a10Z6fDX%2Fo22O2mdmWyCJvt7afc8jGDcIfYldhCWITfCdpw0xEUHQRrAyBxehb91TxMOZIyuIOZiwqXNPMN%2BhZwikp10rKEDxrCi4OiZiDRMcHGLULRToa%2B6Ju%2FGh%2F%2FKXnaKCpL0bJlg9WfmLNxA3hwPne%2FEjKXGncNZMUbKjdp9etntsUMUm6JmUhhB2Qz%2FtSzyoNe5fxdsbKLUQwp6icxwY6pgHCK0EIihSj2pbd3umq%2Bt%2BcyiFBdiY17urQPIrByXavZqIRemcX5%2BvvrTPoebsPqvwC3v%2Bw7kateaEO5OSWa%2BWKzx%2BT5JRK8gGv90T0v3mEZ5Xu5aSYEU1GGm1tYBaUeFZLOtnZDVC7gK8zO%2FV2CiueOPfjLpOpv%2BO%2BArSUvg6UTInVz01109X2pWy8wDHjSG0NNNvmktrC4Q4IUGUBPEPepfbTgx1Y&X-Amz-Signature=09c3c4c3b8f4ad565496ab469d7886c963b2df1374e2494c187ba61799b7f0a8&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

