---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466YWBR7P7F%2F20251008%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251008T080059Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEB8aCXVzLXdlc3QtMiJIMEYCIQC4PJjKl8DIRLjVGzAbgwYu2Wfs%2F0Jel67huYj7i%2FOAvAIhAKgGSry9aERvGcpTx7YUQRGxl1KK%2B%2BF3C6To2VrpxRE4KogECLj%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1IgzZpnw7uqgAJU3miWcq3APAW69J8Ld5dU1sCs8JPjoWdBdKe1qw%2BFRGVoLrLUY6MZrUtYx6s2YovwZvTo4Cugew6HSQNDF%2BRnSdfSCFOGaLsWdLdxf8AXHB6sjeulxEdEqPIJaqlTtilI8IBlDJqjtGeYVhtc589ojk3zqnWXesON3SR9xgdDAcmo22M%2FO8gNbTDgVmHQ9vLu1JDRDOQC3NnOwadoUzeKnV6C9sClyMN2Jx7TJrHxLXiM2GhzwjfYwqr0Jt5zJxHipG5TSvojquvTB0GJT0lzugpCmlMWThhE7cWrP8YkICY3Q5%2BnFteUn8mC0PGTJLqbKLDg73GbmmPAFCZAkc4InEBIWJcaR7HQUhAEbx8KSh2oJwunAhsQu0%2BqCzwpk%2F9qf2dVeltZQnjMJmq7z0wjElpgHngRai5LLdPA7IqUvc6geO3JejgISyEl2pYJ4mEw6Y3%2BX6KWVc%2FdlRnGILWuXpz1w2Vs5QutalbbGGs9N8bsmoxj%2F3MbYuGfHLnVonHfWEkMEFlZaxN7g701AMLqMVRu0Aq3GuBw8%2FhOvn4ADv1JCAuUvIGEeqeFLXvYdVoOlOBAYIxPeMansHoc8GwVYOCNiBZRorTRNv8ukFxxAhad8nXzBxaiqoyKzc8rkwUEWFWDCsjZjHBjqkAV%2BFHWuN9xM43yX%2FyFScn9ddAMh8IltVi%2Fti1AtYi%2Bq7I1Atqu5lYrNZogJ5OjiW9%2FM%2BLf923neVKpWtnmyAY0rqT1AeNvfXxNZokQPl9dJACLDzxw01utT4rNcKPnkfmM5ODsj2zRUVPRBSgOvLAFowWdanX8dJhq4TM1G%2BGGQ8sFkRsTzDY3PBBtCn6CiawFIDDcvlnwn7yx69ZYuTcdNv7bvU&X-Amz-Signature=34c4368fb9f1c8c7c5237c42761c272bd57dbb89bacd6975abdfc4d97925f887&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

