---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4663IW4DX2U%2F20251125%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251125T070055Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEJ%2F%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJHMEUCIQDRHBoPktg04TKMOXxhmoxpE3vJhZAyv7BQcCb1%2FTnKswIgBxXBgxWio%2FEUbGgtcVC8JAEE3UxfL8CMFNM%2B0dytbZcq%2FwMIaBAAGgw2Mzc0MjMxODM4MDUiDPnVOl09xV629S7OvCrcA%2F7Zbjj7T29nrIoMbrrxPR7LlH%2B6RrHwJ9fj5O08MT04HLfbbx%2BNokrwZYBD1iCWQpSFeeHHS7jb3sd7ijILPv3u9SJ3gLrg45Hk%2BN8xF7vIgp0kqWcH%2B39L1li6amOkR3azW3Dg2oWlL%2FRQ5koXamy0%2FKBYyIBdq%2FDy3ZXCcKFvpanBEuU6sfHaWrp1Nq6H0uqDDd%2BIHa24qiOIvvwrNzRbh%2Fiq4O%2Bb8Hh7WCoJSXzqNDHa4T9l2%2B8BC7BQEeIweWzG5yphywzbPZquYFg1shQ1l%2FJrHC3z7gXpnO5c5lAWpK73vAoF7xewU07cR2wWppDrdxtPuy3QEuYyI3pKUtD18Gf8FlDm2GTcl1a6IKV7O8mmcx6NSHQCc1aR%2B1XaZrXasqbZdZJlhOmAxTiUTSWhcysbZ%2FL7H3Hsf7T3Vy4l5vOOfT82fojZWe8Zl1cPB4d9qBoiAaLnjmkbCsh%2BkjiPAHmMIb3z8zaBQHVbwvZc9XWd0p9%2FYZpW6wnsZ3EEM9AOEVL3I69yj%2B7phoJnmLsywBGKmtqgNILg%2BLVAdu4krB1oZ2UaeuCZGWWKBYu1KWMv%2F4U9uv99s6XMuqgMrHBN%2FuCoe3OSzNhlXcNC9AoUJ89Ces90NJ7XCpNsMM2elckGOqUBTqhZdEFGPQpK8s0IBGwrG9EJElZUg9R2wTjCoylScNRtXro%2BjNzvXyOKRFWSkUuVT%2ByBpzECp58070eKjs8L8Cp9qWUKdUOCIkQJlqjCrRSTjsEIpQysAzTNyFAZn0wwWgUcAs85eTWgWEYS55RL2pAx8zkhY5wGmWfxbQj%2FgIXQHDpbLxWtDUNlpWJqwCl6WUftOD7hqii9LTcZgNa6zTTz4pq7&X-Amz-Signature=193701ec0324d56653d75190553c2d621ba1bf95e4a821d35c6e7aca474cf3d0&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

