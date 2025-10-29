---
categories: 业务开发
tags:
  - mysql
sticky: ''
description: ''
permalink: ''
title: mysql 中使用 json 类型的字段
date: '2025-09-14 17:39:00'
cover: 'https://prod-files-secure.s3.us-west-2.amazonaws.com/143cad91-961b-48b0-82dc-78fbb6eb5abe/deb71c7a-9910-435b-b686-00d0786e45d3/51711470_p0.jpg?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Content-Sha256=UNSIGNED-PAYLOAD&X-Amz-Credential=ASIAZI2LB4662O7TMRVB%2F20251029%2Fus-west-2%2Fs3%2Faws4_request&X-Amz-Date=20251029T120039Z&X-Amz-Expires=3600&X-Amz-Security-Token=IQoJb3JpZ2luX2VjEBsaCXVzLXdlc3QtMiJHMEUCIQD8Yo5hH%2FtL%2Ffuc%2F21HRv0jGUm%2Fuk1XwydeRtZNR9%2FGkAIgNZl%2Bfe65qMcZALZlwfJpQw8DiMgNYQbXjyrZcCH0eokqiAQI1P%2F%2F%2F%2F%2F%2F%2F%2F%2F%2FARAAGgw2Mzc0MjMxODM4MDUiDLNIlAigwRzllmlaYircA3xa8bP2cW12bBmzrHJC6vz3xabu1l0YEs4vimeogBpFhaaJVDe7qh46wPodXha9vjrjdtSbHCVtLr%2BJJELawkcBlDXoTYhC8yQyPgoa4STObhcKtpwNrPeauSJlayYGPTFHhomK2WOGIjCTyS9KPAHyiqx8%2Bhs53qVXpbCYoflUlS3F3gTZ6F0I6EX%2Ff7CcU0no44iLrcbKWvw0g4q0UtxU%2BbZXP9F9qxDsTU7L%2F%2BbFn2Gv%2FkBBpEh5w6MIleqRgP2g5hC9OJkXbIDtSb6DQaUstB9Ze6ooJ%2B6%2F49nNQFTFSiyC6ZNa21%2BaBvnWtUEOYvcYKC14WKR1GjVmWBje9KOf2M8mAeMulwMXaFX7LrClbJVfSdz0X%2FiVaTzPmU45XIyv1XcSad%2BxVrya%2BS7qwJbkkRiIHLDBIp8uRC651GVAxywQDakNm%2F0cawBQKZ%2BpIxA1yu%2B7AN7beO%2F%2FRyAnGf7RsAyS7r9DcnsdU%2B%2BQb%2BWq3b0jXK%2F4olzUegw%2B8zko4AYommtZ3ZEZv%2Fa2D7hMJT%2F2nifZBzWAP1PfBCGatXavpeNWruEm8MA8RepARL4fK6qPAGRF%2BIp0YVfYmwjPZJjKcNCowzGUWKmyLCnyFK%2F3L38imgds3ePbOylgMK7oh8gGOqUBJrm1F2ME%2Bd9v9%2B4QIk%2BF3POTziMpyU1JkJ33GisfdF6x140gmRVVWXpeZQbvMKvCE6qneXekWy%2Fb39HEIwmjW0Ndxw1RB4VKESE09bIAZSD5wkdrtb%2BOpENohZJybe3%2FWl4%2FJPIFLDOW29s%2BDT%2FNckGk%2Fp7rPjrT9nvrsRzQ%2FCEGerC03A3ksas4otFHUKJC9f%2FnRYczmrLT3YLCGtxnNYc9Va8M&X-Amz-Signature=6ad9f5bf7199adc25b3bc779b3a96f59cb8eddb3996851f8a253342620ec5cce&X-Amz-SignedHeaders=host&x-amz-checksum-mode=ENABLED&x-id=GetObject'
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

