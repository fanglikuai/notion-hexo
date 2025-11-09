---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466QDBANFEY%2F20251109%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251109T220036Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjECkaCXVzLXdlc3QtMiJIMEYCIQDiyAuXe9AIZVHoMgY%2BL7KUBmeS4Z0r0c7Dt6ppC1DFmQIhAJD%2B9pzaVXglPw5PC%2FFCgMtxhoG0%2FMQP2FETT1HuYlxmKogECPL%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgyHqkNV%2B0xaLW6ItrYq3AOJNZ3uQLGqvSrJKp7o7nsyLRQ3fS47fDr%2BV5hVM1hDrkQHKdFV0jyGJUnrPZup6%2BohsAngUnIKN8hP2PPqHHlUtdQbk9tKe39eoOc0BXj%2BINJUuhUULxciKbsPE0DEbVGOBSKhvNWXJ00Qtet2JoH4jcwXRdwvSozHC6DmOcF0pObn45%2FTrsXppavnVk09Y2TYTZcOznt%2F5OL%2B2slhsjBo3kG2vPCoE4OxIWfo%2B0HowvYEewcEE2OAEEgooVD695CHdDjWlDsh883vn1KQPdwSXpf6l8myMjawWLcI8MFC4bNXVum3Gcp3D3%2Biju5%2BgR0dDhinHVTY0kzAPMedqEiH9H26MVL7bowUIPiY0yaTq34gCI1DaVslD9XZDN%2BQCG4r%2FLLCNBxaS3P%2Bu%2B%2Fv033pnhZEaXW6keIpmIZcJriABUWXtOLtHrRgezyqUx0jqp%2FJlZQ7qYM0qXLO2VekXKsVMkvL7PXw0kVsYIcBsAPXJAn2np2MLvi49axRqddyNIKpwEH6Z%2BW%2FxaDCgQFnrWGO1F95hsuIPuV4pXvrUJV7C3wjj5fpRJlswBdLGjhVLEvdwj3XUoxKruX4nsZgIsfQGMh1kVnlDvLaiMG4ZxnWNWotRDKsL0f%2BvcjpDzDPgMPIBjqkAejCF4iD%2BwJQ%2FJsV8iAC7XKO%2Fj9cEdXAb4t3krfYN%2F%2Bz2zxWV5XrljwxKCRYHei%2BL04SaulRPeWZYukt8zSypkbVXqe%2FLPTg6iJfzRFYTqZvgYrnNP0hXnVwV6JmPvDuLuZ8UTZ7Ihy5uecVNz%2BnjbWD2LjaLH35bbabck48fHhJeHYgae4TwePnzQfguM%2Fb2jczOk6RUUQrI5YERdwPdeNCnVw2&X-Amz-Signature=f77a0bfc8669bb2dab29a3f16b6ce3b15fee10ff6a65e8fdae0c1cea9be50591&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

