---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB466SNBYOSXC%2F20251028%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251028T020053Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEPr%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEaCXVzLXdlc3QtMiJIMEYCIQDIXI8otClm5WSPLbVuE%2FNfd4VEddDmYvNE9PRe3RFtswIhAJxPoM2l0xfm2yNZFHxXwc1R6fXf0vSmPqMjieQwUzCvKogECLP%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FwEQABoMNjM3NDIzMTgzODA1Igwdb%2B0p0GHG1kEUUZMq3APvOzLLyRYVNHstvJ%2F2JnyBLklVK4%2BHj6a32aNzqrcnHOd2Q2kbQK2W5UQzZq5ByixuX%2B8PtXrJLw1gjX1Qvpm%2F%2FQcLkEKz1vo1PpSga4oLtKtqsOj5izjfyhH57La1XXhC86PMkMKy1JXk3%2BFt8N%2FSTJHBr7J%2BKimnxs6cDrgoW6qU648phbWQsL8r5gPk%2FL1ee06DK0p46MVNDsqCgNgF%2BJXDHTSpJ0%2F8yhiWBpIFwl3OpySBjqPZv5QG7xc4JCIrHyfr055FDE7a%2B%2BiExddDYF%2BfoknFr0pTaS0cnkSNTSRUAo%2FptYt2wAnwI25I9cvMc0V%2BZESrsjBWcsfNLam8cfM0PP7l9IfrqYmD2IRNlgbx%2BVeWjUUYS7w74KBrfp1KjM9RGqiqOKCu3GLE0Uxf1IGnOP3KnJPR3anN3RZRXTxS%2BdcdMjO5vYWylzRh6Q5DZODh8ehftRQgg4sUJ9tYJSaxDDMbVHPeKH4MSkO6w4dhyLgzAmBW%2F%2BHagKV6V2CVodSsnHBtbdhxo9kTNJnuIL9T5wjNk0XE2PzBZXItsfqwVcVc2%2BqayfP%2FLEFPZ5xtSWjS4WGt9JcXspkD%2BzaprHF%2BEoXZ46gWJu7CtuxyTt6EiWmPZDF4t2L0ezChvYDIBjqkAbcG%2BB27RzJ9o7t5tQ%2B%2FzFYgzPbp%2BM2A%2B3jcwND177nr0d4867LwzD0hlQyk1D1XXJdRCT9uC5orHqrVOelKLLdtZWhZPWbtQq1PB6SEYDWFEq8lcHvOgUIP%2FfdLX2kZs3s5bQp%2ByDNQ6RrNI7nvUA7B05pY8VKrSvUUiZjwpMXwioF6g42s91laFXof8SOYkyTQ%2FOsDMbCCx5mqfcCeGvDWdKjU&X-Amz-Signature=075c2d813b12914e92ce8c60f6fb1f82a440a23e3f59af75f47e9964b3a4a3ec&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

