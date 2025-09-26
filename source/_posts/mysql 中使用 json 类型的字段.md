---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466WCJDUWUE%2F20250926%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20250926T180050Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEAoaCXVzLXdlc3QtMiJHMEUCIHQA%2BcH%2BsLWT6RPwxCPQsbAO0VpN8hONrDb1lXiWjh51AiEAwZrjR3N1zMYnGCaBC8RdJDNWCghVH5AB24Se5bctbh8qiAQIk%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDEYaR5SFyKw7%2BWmcpSrcAw%2Bl8%2BuEgmD8ELIR9dTtJgRniiRI0PYgOtgapnts12eYqoVNfD22o2H6HwtWi98SCQtaP566U4sRTY1bMad2gIRpTjeY5KE%2BYv7hUxSKyeuAvB%2BRp%2BvSv4NpzF3Po5eTnORCPjhLJsdrKFEM2T16X1uJASCIASfr6uzVWc5HTlNuy8kH%2BEQsB82%2FryJHN8uOentbWvubJmHHOqIK4q24Y9xAZI9IxJ%2BkK0fXaopagETTKzMs7wEi5AcVOQUDmYnCI%2BjEWNQzRm5pOWYUdwHHSd6s6xSQk5t1ogPrqFbnV9ioBWEwERzsioiUrmaK4yBBICxKJBRbja2n2XbP7d93V%2F13RETRigr74Md%2Bd2q3eMeBIQmn0SeqDwdQAfU68%2FCk5WBjXpCX4lXRBzXeueSHWOgdNu4c%2B8rqnIZ9g%2FZExTkxhSVBpHeLvuPD0MQgZxSE4LiUYfLs80rQq06FWjgIeH7f58NL4%2BGyQwf7WLY%2F39gmyAUjWlCqZ8ShLfxAFToaySpb7rdBSsxnx2wPR7YNLL9ZpsBYVzmSRN3Bry6mgXPJNQjtzzTmod8bWBZfy8c8qpvJMCC3nNAabN4oVOCRWTCtsa47CMWKnXaA%2BMTy5z4DXFclRXSvRbwEw2q0MMyg28YGOqUBnkWlP3IVjXVIKJntC8%2Fb9d3Hrw%2Fu%2BDTnnqib1DACjW53WrmiZQ1GyPYJFzpjp%2FRQGzZd8RjyIBTr2CQT%2BN55hzzu9hAhrnt6wKrzHrp%2BS1e%2FxPSlzRqL9XlsGMXg9ENG%2BnBOe%2F1Usho0Sc6UsE7uFuv2wkGZbpW3v4aLl7FIZ2hkRvCiip3ns%2BzD%2BqS5A%2BA7F4iVlc%2Buza6TJgUTDGwXHwLp4AAS&X-Amz-Signature=00bf38f2c96f545da8c5c46e7986f4bd7fb37397757b5522d32c8e90df0a9571&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

